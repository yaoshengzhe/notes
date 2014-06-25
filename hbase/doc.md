## HMaster构造函数

HRegionServer构造函数, HRegionServer的RPC端口默认是60020，master的RPC端口默认是60000
HRegionServer的Jetty(InfoServer)端口默认是60030，master的Jetty(InfoServer)端口默认是60010

1.1 获取当前运行HMaster的机器的地址
1.2 生成HBaseServer对象用于接收RPC请求，并启动HBaseServer的相关线程
1.3 生成ZooKeeperWatcher对象, 在构造函数中生成这些持久结点: /hbase, /hbase/unassigned, /hbase/rs, /hbase/table, /hbase/splitlog

	ZooKeeperWatcher管理下面10个结点:

	baseZNode              "/hbase"
	rootServerZNode        "/hbase/root-region-server"
	rsZNode                "/hbase/rs"
	drainingZNode          "/hbase/draining"
	masterAddressZNode     "/hbase/master"
	clusterStateZNode      "/hbase/shutdown"
	assignmentZNode        "/hbase/unassigned"
	tableZNode             "/hbase/table"
	clusterIdZNode         "/hbase/hbaseid"
	splitLogZNode          "/hbase/splitlog"
	schemaZNode            "/hbase/schema"

	这6个结点在ZooKeeperWatcher构造函数中生成
	baseZNode              "/hbase"
	rsZNode                "/hbase/rs"
	drainingZNode          "/hbase/draining"
	assignmentZNode        "/hbase/unassigned"
	tableZNode             "/hbase/table"
	splitLogZNode          "/hbase/splitlog"
	schemaZNode            "/hbase/schema"

	这4个在不同地方生成
	rootServerZNode        "/hbase/root-region-server"
	masterAddressZNode     "/hbase/master" //在HMaster中建立，并且是一个短暂结点，结点的值是HMaster的ServerName
	                                       //见org.apache.hadoop.hbase.master.ActiveMasterManager.blockUntilBecomingActiveMaster
	clusterStateZNode      "/hbase/shutdown"
	clusterIdZNode         "/hbase/hbaseid" //在HMaster.finishInitialization方法中调用ClusterId.setClusterId建立，结点值是UUID

1.4 生成MasterMetrics对象

## HMaster.run

2.1 生成ActiveMasterManager对象，如果此HMaster作为一个备份(backup)，那么需要等到集群中有Active Master时才往下调用blockUntilBecomingActiveMaster, 并且调用blockUntilBecomingActiveMaster也会阻塞，直到它变成ActiveMaster。与此同时，在blockUntilBecomingActiveMaster中会创建短暂结点"/hbase/master"，此节点的值是HMaster的版本化ServerName(也就是version+ServerName), 此结点用于协调region server的启动，只有"/hbase/master"创建好后，region server才能往下进行。

2.2 调用HMaster.finishInitialization

    2.2.1
	生成MasterFileSystem对象
	建立由hbase-site.xml的hbase.rootdir属性指定的目录(如:file:/E:/hbase-0.90.4/tmp/hbase-db/hbase)
	调用FSUtils.setVersion在hbase.rootdir目录中建立一个hbase.version文件，并写入版本号(HConstants.FILE_SYSTEM_VERSION=7)
	判断-ROOT-分区是否存在，不存在则调用MasterFileSystem.bootstrap来创新-ROOT-和.META.

	最后创建file:/E:/hbase-0.90.4/tmp/hbase-db/hbase/.oldlogs目录

	2.2.2
	如果持久结点"/hbase/hbaseid"不存在则创建它，否则不创建，同时每次master启动时都会把此节点的值设为hbase.id文件中的值

	2.2.3
	生成ExecutorService (TODO)

	2.2.4
	生成ServerManager (TODO)

	2.2.5
	initializeZKBasedSystemTrackers

	  2.2.5.1
	  生成CatalogTracker, 它包含两个ZooKeeperNodeTracker，分别是RootRegionTracker和MetaNodeTracker，
	  对应/hbase/root-region-server和/hbase/unassigned/1028785192这两个结点(1028785192是.META.的分区名)
	  如果之前从未启动过hbase，那么在start CatalogTracker时这两个结点不存在。
	  /hbase/root-region-server是一个持久结点，在RootLocationEditor中建立

	  2.2.5.2
	  生成AssignmentManager 

	  2.2.5.3
	  生成 LoadBalancer

	  2.2.5.4
	  生成 RegionServerTracker: 监控"/hbase/rs"结点

	  2.2.5.5
	  生成 DrainingServerTracker: 监控"/hbase/draining"结点

	  2.2.5.6
	  生成 ClusterStatusTracker，通过它的setClusterUp方法创建持久结点"/hbase/shutdown"，结点值是当前时间，
	  如果结点已存在(master可能未正常关闭)，那么此结点的值不更新。

	
	2.2.6
	生成 MasterCoprocessorHost

	2.2.7
	startServiceThreads()

	启动服务线程
	(MASTER_OPEN_REGION、MASTER_CLOSE_REGION、MASTER_SERVER_OPERATIONS、MASTER_META_SERVER_OPERATIONS、MASTER_TABLE_OPERATIONS
	这几个只是生成Executor，并未正式启动, 正式启动的有LogCleaner，和基于Jetty的InfoServer(端口号默认是60010))
	
	2.2.8
	等待RegionServer注册

	2.2.9
	splitLogAfterStartup (TODO)

	2.2.10
	assignRootAndMeta

		2.2.10.1
		processRegionInTransitionAndBlockUntilAssigned
		先看一下分区正在转换状态当中，如果处于转换状态当中则先处理相关的状态，并等待体处理结束后再往下进行。

		2.2.10.2
		verifyRootRegionLocation

		2.2.10.3
		getRootLocation

		2.2.10.4.A
		expireIfOnline

		2.2.10.4.B
		assignRoot

		先删掉"/hbase/root-region-server",不管它存不存在，KeeperException.NoNodeException被忽略了

		写入EventType.M_ZK_REGION_OFFLINE、当前时间戳、跟分区名(-ROOT-,,0)、master的版本化ServerName
		到/hbase/unassigned/70236052, payload为null，所以不写入

		RegionServer修改/hbase/unassigned/70236052的值，
		写入EventType.RS_ZK_REGION_OPENING、当前时间戳、跟分区名(-ROOT-,,0)、RegionServer的版本化ServerName


	2.2.11
	MetaMigrationRemovingHTD.updateMetaWithNewHRI

	2.2.12
	assignmentManager.joinCluster()
	把meta表中的分区读出来，然后分配到Region Server,
	meta表只有一个列族：info，存入meta的行有三列: 
	regioninfo、server、serverstartcode，
	其中regioninfo对应HRegionInfo，
	server对应ServerName的host和port(例如:myhost:60070)
	serverstartcode对应ServerName的startcode(一般是时间戳)。

		2.2.12.1
		rebuildUserRegions()

			2.2.12.1.1
			调用MetaReader.fullScan 从meta表中取出所有的分区，得到一个List<Result>，
			调用MetaReader.parseCatalogResult，解析每个result得到Pair<HRegionInfo, ServerName>，
			其中HRegionInfo由regioninfo列的值反序列化得来，ServerName由server、serverstartcode两列的值反序列化后组合而成。

		2.2.12.2
		processDeadServersAndRegionsInTransition


HRegionServer在reportForDuty()中向HMaster报告自己启起来后，
接着调用handleReportForDutyResponse把自己挂到zookeeper的/hbase/rs节点下(是个短暂节点)，

之后HMaster就可以从/hbase/rs节点中得知有多少个HRegionServer了。


0 HRegionServer构造函数

HRegionServer构造函数, HRegionServer的RPC端口默认是60020，master的RPC端口默认是60000
HRegionServer的Jetty(InfoServer)端口默认是60030，master的Jetty(InfoServer)端口默认是60010

1 run

	1.1
	preRegistrationInitialization

	1.1.1
		initializeZooKeeper() 此方法不会创建任何节点

		. 生成ZooKeeperWatcher
		. 生成MasterAddressTracker 并等到"/hbase/master"节点有数据为止
		. 生成ClusterStatusTracker 并等到"/hbase/shutdown"节点有数据为止
		. 生成CatalogTracker 不做任何等待

	1.1.2
		initializeThreads()

		. 生成 MemStoreFlusher
		. 生成 CompactSplitThread
		. 生成 CompactionChecker
		. 生成 Leases
	
		1.1.3
	参数hbase.regionserver.nbreservationblocks默认为4，默认会预留20M(每个5M,20M = 4*5M)的内存防止OOM

	1.2
	reportForDuty

		1.2.1 getMaster()
			取出"/hbase/master"节点中的数据，构造一个master的ServerName，然后基于此生成一个HMasterRegionInterface接口的代理，
			此代理用于调用master的方法

		1.2.2 regionServerStartup

			1.2.3.1
			用rs的端口(默认是60020)、startcode(rs构造函数被调用时的时间戳)，now(当前时间)这三个参数调用master的regionServerStartup.

		1.2.3 handleReportForDutyResponse
			1.2.3.2
			regionServerStartup会返回来一个MapWritable，
			这个MapWritable有三个值:
			"hbase.regionserver.hostname.seen.by.master" = master为rs重新定义的hostname(通常跟rs的InetSocketAddress.getHostName一样)
															 rs会用它重新得到serverNameFromMasterPOV
			"fs.default.name" = "file:///"
			"hbase.rootdir"	= "file:///E:/hbase/tmp"

			这三个key的值会覆盖rs原有的conf

			1.2.3.3
			查看conf中是否有"mapred.task.id"，没有就自动设一个(格式: "hb_rs_"+serverNameFromMasterPOV)
			例如: hb_rs_myhost,60050,1323525314060

			1.2.3.4
			createMyEphemeralNode

			在zk中建立 短暂节点"/hbase/rs/myhost,60050,1323525314060"，
			也就是把当前rs的serverNameFromMasterPOV(为null的话用rs的InetSocketAddress、port、startcode构建新的ServerName)
			放到/hbase/rs节点下，"/hbase/rs/myhost,60050,1323525314060"节点没有数据。

			1.2.3.5
			设置conf中的"fs.defaultFS"为"hbase.rootdir"的值(conf之前可能没有"fs.defaultFS"属性)

			1.2.3.6
			把"hbase.rootdir"的值保存到rootDir字段，
			生成一个只读的FSTableDescriptors

			1.2.3.7
			setupWALAndReplication

				1.2.3.7.1
				得到oldLogDir = "hbase.rootdir"的值+".oldlogs"，例如: file:/E:/hbase/tmp/.oldlogs
				得到logdir		= "hbase.rootdir"的值+".logs"+serverNameFromMasterPOV，
											例如: file:/E:/hbase/tmp/.logs/myhost,60050,1323525314060

				(备注: 假设"hbase.rootdir" = "file:/E:/hbase/tmp/")

				如果logdir已存在，抛出RegionServerRunningException


				1.2.3.7.2 (TODO)
				判断是否使用Replication，默认为false，可通过"hbase.replication"参数设置

				1.2.3.7.3
				instantiateHLog

					1.2.3.7.3.1
					getWALActionListeners

					只有下面两个数实现了org.apache.hadoop.hbase.regionserver.wal.WALActionsListener
					org.apache.hadoop.hbase.regionserver.LogRoller
					org.apache.hadoop.hbase.replication.regionserver.Replication

					LogRoller会加入WALActionsListener列表中
					"hbase.replication"参数的值是true时，Replication也被加入


					1.2.3.7.3.2 (TODO)
					生成一个新的HLog
					在他的构造函数中会建立oldLogDir和logdir两个目录，
					prefix字段的值是serverNameFromMasterPOV经过URLEncoder.encode(prefix, "UTF8")后的值，
					然后在logdir目录中生成一个新的日志文件，
					日志文件名是prefix+当前时间戳，比如: myhost%2C60050%2C1323525314060.1323527965276

			1.2.3.8
			生成RegionServerMetrics

			1.2.3.9
			startServiceThreads

			RS_OPEN_REGION RS_OPEN_ROOT RS_OPEN_META
			RS_CLOSE_REGION RS_CLOSE_ROOT RS_CLOSE_META
			这6个并未正真启动，只是生成Executor。

			启动的线程有:
			LogRoller
			MemStoreFlusher
			CompactionChecker
			Leases
			Jetty InfoServer (可通过"/rs-status"和"/dump"这两个url来访问rs的相关信息)
			Replication(待确定 TODO)
			SplitLogWorker

		1.2.4 周期性(msgInterval默认3妙)调用doMetrics，tryRegionServerReport

			1.2.4.1
			isHealthy健康检查，只要Leases、MemStoreFlusher、LogRoller、CompactionChecker有一个线程退出，rs就停止
			
			1.2.4.2
			doMetrics

			1.2.4.3
			tryRegionServerReport
			向master汇报rs的负载HServerLoad

## Compaction

1.1 compaction 配置参数

	* maxCompactSize (hbase.hstore.compaction.max.size, default: Long.MAX_VALUE) - upper bound on file size to be included in minor compactions
	* minCompactSize (hbase.hstore.compaction.min.size, default: MemstoreFlushSize) - lower bound below which compaction is selected without ratio test
	* minFilesToCompact (hbase.hstore.compaction.min, default: 3) - lower bound on number of files in any minor compaction
	* maxFilesToCompact (hbase.hstore.compaction.max, default: 10) - upper bound on number of files in any minor compaction
	* compactionRatio (hbase.hstore.compaction.ratio, default: 1.2) - Ratio used for compaction
	* offPeekCompactionRatio (hbase.hstore.compaction.ratio.offpeak, default: 5.0) - 
	* throttlePoint (hbase.regionserver.thread.compaction.throttle, default: 2 * maxFilesToCompact * MemstoreFlushSize) - 
	* majorCompactionPeriod (hbase.hregion.majorcompaction, default: 7 days) -
	* majorCompactionJitter (hbase.hregion.majorcompaction.jitter, default: 0.5) -

1.2 Compaction接口: CompactionContext - 提供了必要的信息用于Compaction

1.3 CompactionPolicy: Compaction决策类，通过继承该类来实现个性化Compaction

2.1 CompactSplitThread: 负责对Region进行Compaction。如果Compact之后Region过大怎么办？Split! 所以该类也会对Split进行处理。每个RegionServer上会有一个该线程。

3.1 HStore.compact: 真正工作的地方。

# HBase Server

## RPC

1.1 RpcScheduler

## RPC - Source Code
1. BufferChain: 一堆ByteBuffer的集合，提供简单接口来实现写指定chunkSize的数据到channel中。
  其中write方法写的很烂，简单来说就是找到remaining不为空的buffer，然后再找出从这个buffer.position()开始
  的连续chunkSize长度的数据(如果该buffer数据不够，就从下一个buffer找)。然后调用GatheringByteChannel.write，
  写完后注意恢复最后一个buffer的limit即可(具体实现看code)，总之写的烂。

# HBase Client

1.1 ClientScanner <- AbstractClientScanner <- ResultScanner
