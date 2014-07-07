## visual vm -- remote monitoring

On remote machine:

    1. Create policy file: jstatd.all.policy with following content:
       
       grant codebase "file:${java.home}/../lib/tools.jar" {
         permission java.security.AllPermission;
       };
       
    2. Launch jstatd: sudo jstatd -J-Djava.security.policy=jstatd.all.policy


JMX_OPTS="-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9010 -Dcom.sun.management.jmxremote.local.only=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false"


