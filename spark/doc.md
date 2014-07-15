## Broadcast

var broadcastedVar = sc.broadcast(YOUR_OBJ)

目前两种实现HttpBroadcast和TorrentBroadcast

抽象类: Broadcast
trait: BroadcastFactory

BroadcastFactory也有两种实现HttpBroadcastFactory和TorrentBroadcastFactory

用户通过类BroadcastManager来进行操作。
通过config: "spark.broadcast.factory"来指定所需要用的factory类

BroadcastFactory负责创建Broadcast实例，BroadcastManager通过configuration来觉定使用哪个BroadcastFactory

#### HttpBroadcast实现一些细节
通过使用jetty来传输数据，每次创建一个新的broadcast variable时，如果不是本地(isLocal == false)
spark.local.dir

spark.buffer.size：读写数据时Buffered[In/Out]putStream的buffer大小，默认：65536
spark.broadcast.compress：是否采用压缩，默认：true
spark.httpBroadcast.uri: http server uri地址, 当调用HttpBroadcast.createServer(conf: SparkConf)时被初始化
spark.io.compression.codec: 压缩编码，默认：LZFCompressionCodec

每次创建一个broadcast variable时，如果variable不是本地(isLocal == false)，则将变量写入到本地文件中(broadcastDir/broadcast_ID，其中broadcastDir是一个temp dir，在创建server时初始化)。

当每个client需要这个变量时，通过地址serverUri/broadcast_ID 来从server读取该变量。

#### TorrentBroadcast实现细节 (TODO)

## Executor
## Storage

