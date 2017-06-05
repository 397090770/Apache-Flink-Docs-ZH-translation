---
title:  "YARN Setup"
nav-title: YARN
nav-parent_id: deployment
nav-pos: 2
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

* This will be replaced by the TOC
{:toc}

## ���ٿ�ʼ

### ��YARN������һ�����ڵ�Flink��Ⱥ

����һ��ӵ��4��Task Manager��yarn�Ự��ÿ��Task Manager��4gb�Ķ��ڴ�:

~~~bash
# ��flink����ҳ��ȡhaddoop2��
# http://flink.apache.org/downloads.html
curl -O <flink_hadoop2_download_url>
tar xvzf flink-{{ site.version }}-bin-hadoop2.tgz
cd flink-{{ site.version }}/
./bin/yarn-session.sh -n 4 -jm 1024 -tm 4096
~~~

�ر�ָ����-s������ʾÿ��Task Manager�Ͽ��õĴ���ۣ�processing slot�����������ǽ���Ѳ��������ó�ÿ�������������ĸ�����
һ���Ự�������������ʹ��./bin/flink�����ύ���񵽼�Ⱥ�ϡ�

### ��YARN������һ��Flink������

~~~bash
# ��flink����ҳ��ȡhaddoop2��
# http://flink.apache.org/downloads.html
curl -O <flink_hadoop2_download_url>
tar xvzf flink-{{ site.version }}-bin-hadoop2.tgz
cd flink-{{ site.version }}/
./bin/flink run -m yarn-cluster -yn 4 -yjm 1024 -ytm 4096 ./examples/batch/WordCount.jar
~~~

## Flink YARN �Ự

Apache [Hadoop YARN](http://hadoop.apache.org/)��һ����Դ�����ܣ�����һ����Ⱥ�����ж��ֲַ�ʽӦ�ó���
Flink ���Ժ�����Ӧ�ó���һ���� YARN �����С�����Ѿ�������YARN���û��Ͳ�����������װ�κζ�����

**Ҫ��**

- Apache Hadoop�汾����2.2
- HDFS��Hadoop�ֲ�ʽ�ļ�ϵͳ������������Hadoop֧�ֵķֲ�ʽ�ļ�ϵͳ��.

�������ʹ��Flink YARN�ͻ���������ʱ���뿴��[������̳](http://flink.apache.org/faq.html#yarn-deployment).

### ����Flink�Ự

�������½���ѧϰ���������yran��Ⱥ������һ��Flink�Ự.

һ���Ự����������Flink����JobManager and TaskManagers����������Ϳ����ύ�������Ⱥ���У���ס��һ���Ự�п�������
�������

#### ����Flink

����һ��Hadoop�汾����2��Flink�����ɴ�[������ҳ](http://flink.apache.org/downloads.html)��á���������������ļ���
��ȡ���ذ��ķ���:

~~~bash
tar xvzf flink-1.4-SNAPSHOT-bin-hadoop2.tgz
cd flink-1.4-SNAPSHOT/
~~~

#### ����һ���Ự

ʹ����������������һ���Ự��

~~~bash
./bin/yarn-session.sh
~~~

������ĸ������£�

~~~bash
ʹ��:
   Ҫ��:
     -n,--container <arg>   YARN���������� (=taskmanager�ĸ�����
   ��ѡ����
     -D <arg>                        ��̬����
     -d,--detached                   �������루�ύjob�Ļ�����yarn��Ⱥ���룩
     -jm,--jobManagerMemory <arg>    JobManager Container�ڴ��С [in MB]
     -nm,--name                      �Զ����ύjob������
     -q,--query                      չʾyarn�Ŀ�����Դ���ڴ�ͺ��� (memory, cores)
     -qu,--queue <arg>               ָ��yarn����.
     -s,--slots <arg>                ÿ��TaskManager�Ĵ������
     -tm,--taskManagerMemory <arg>   ÿ��TaskManager Container���ڴ��С [in MB]
     -z,--zookeeperNamespace <arg>   �ڸ߿���ģʽ�£������ռ�Ϊzookeeper������·��
~~~

��ע�⣬�ͻ�����Ҫ YARN_CONF_DIR �� HADOOP_CONF_DIR �������������úã�����ͨ������ȡ YARN �� HDFS �����á�

**����:** �����������10��Task Manager��ÿ��ӵ��8GB�ڴ��32������ۣ�

~~~bash
./bin/yarn-session.sh -n 10 -tm 8192 -s 32
~~~

ϵͳ��ʹ��conf/flink-conf.yaml�µ����á�����������һЩ���ã���ο������ֲᡣ

Flink��YARN�ϣ�������д�������ò�����ֵ��jobmanager.rpc.address����ΪJob Manager���Ƿ����ڲ�ͬ�����ϣ���
taskmanager.tmp.dirs������ʹ��YARN����tmpĿ¼����parallelism.default������۸�����ָ������

����㲻��ı������ļ����������ò����������и���������ö�̬���ԣ�ͨ��-D��ʾ����������ͨ�����·��������ݲ�����
-Dfs.overwrite-files=true -Dtaskmanager.network.memory.min=536346624.

���ӽ���������11�����������ܽ���10������������Ϊ����Ҫ�����1��������ApplicationMaster and Job Manager.

ֻҪFlink������YARN��Ⱥ�ϣ��������㿴��Job Manager�������ϸ�ڡ�

ͨ��ֹͣunix���̣�ʹ��CTRL+C�����ֹͣYARN�Ự�������ڿͻ�������stop��

Flink��YARN�Ͻ�����������������������YARN��Ⱥ�����㹻�Ŀ�����Դ�����YARN���ȳ���Ϊ���������������ڴ棬һЩ������vcores������

Ĭ�������vcores�������ڴ���ڵ�����-s����yarn.containers.vcores�����Զ���ֵ��дvcores������

#### ����YARN�Ự

����㲻�뱣��Flink YARN�ͻ���һֱ���У�������������YARN�Ự���ﵽĿ�ġ������������-d��--detached��
�ڴ�����£�Flink YARN�ͻ��˽����ύFlink����Ⱥ�У�Ȼ��ر����ӡ�ע������ڴ�����£���������ʹ��Flink��ֹͣYARN�Ự��
ʹ��YARN���yarn application --kill <appId>����ֹͣYARN�Ự��

#### �������лỰ

ʹ��������������һ���Ự

~~~bash
./bin/yarn-session.sh
~~~
������չʾ���¸�����

~~~bash
��������:
     -id,--applicationId <yarnAppId> YARN application Id
~~~
��֮ǰ������YARN_CONF_DIR �� HADOOP_CONF_DIR������������������YARN �� HDFS ���ö�ȡ����

**����:** ���������������һ�������е�Flink YARN�Ựapplication_1463870264508_0029

~~~bash
./bin/yarn-session.sh -id application_1463870264508_0029
~~~

ʹ��YARN ��Դ������������Job Manager��RPC�˿ڴӶ�����һ�����еĻỰ��
ֹͣYARN�Ự��ͨ��ֹͣunix���̣�CTRL+C����ͨ���ٿͻ�������stop��

### �ύjob��Flink

ʹ�����������ύһ��Flink����YARN��Ⱥ��

~~~bash
./bin/flink
~~~

��ο������пͻ����ĵ���
�����а����˵����£�

~~~bash
[...]
run������������г���

 �﷨: run [OPTIONS] <jar-file> <arguments>
  "run" ��������:
     -c,--class <classname>           ������ڵ��� ("main"���� �� "getPlan()" ����.jar�ļ�û�������嵥��ָ�������Ҫ.
     -m,--jobmanager <host:port>      ����Job Manager��master���ĵ�ַ. ʹ�ô˲�������һ����ͬ��job����������������������ָ��.
     -p,--parallelism <parallelism>   ���г���Ĳ��ж�. �����ѡ�����ɸ���������ָ����Ĭ��ֵ��
~~~

��run�����ύһ��job��YARN�ϡ��ͻ��˿��Ծ���Job Manager�ĵ�ַ����������£����ʹ��-m����ָ��Job Manager��ַ��Job Manager��ַ����YARN����̨������

**����**

~~~bash
wget -O LICENSE-2.0.txt http://www.apache.org/licenses/LICENSE-2.0.txt
hadoop fs -copyFromLocal LICENSE-2.0.txt hdfs:/// ...
./bin/flink run ./examples/batch/WordCount.jar \
        hdfs:///..../LICENSE-2.0.txt hdfs:///.../wordcount-result.txt
~~~
����������´�����ȷ������Task Manager�Ѿ�����:

~~~bash
Exception in thread "main" org.apache.flink.compiler.CompilerException:
    Available instances could not be determined from job manager: Connection timed out.
~~~

�������Job Manager��web�ӿ��в鿴Task Manager���������ӿڵĵ�ַ����YARN�Ự�Ŀ���̨�������
���Task Managerһ������û����ʾ������ô��Ӧ������־�ļ��м��������ġ�

## ��YARN������һ��Flink ����

�����ĵ��������������һ��Flink��Ⱥ��Hadoop YARN�����¡���Ҳ���Խ�ִ��һ��job������Flink��YARN�¡�
��ע��ͻ�����Ҫ-ynֵ������Task Manager��������

***����:***

~~~bash
./bin/flink run -m yarn-cluster -yn 2 ./examples/batch/WordCount.jar
~~~

��YARN�Ự�������� ./bin/flink tool�ǿ�ѡ�ģ���y��yarnǰ׺��

ע�⣺�����ͨ������FLINK_CONF_DIR����������Ϊÿ��jobʹ�ò�ͬ������Ŀ¼��
ʹ���������������Flink�ֲ���confĿ¼��������ÿ��job����־��

ע�⣺���-m yarn-cluster�͸���YARN�Ự��-yd�������"�ٻٺ�����"�ύFlink job��YARN��Ⱥ�С�
�ڴ�����£����Ӧ�ó��򽫵ò����κ�ȷ�Ͻ���� �ų�ExecutionEnvironment.execute()��������Ϣ��

## ʹ��jars&Classpath

Ĭ���£�Flink����õ���jars����ϵͳ·����������һ��jobʱ�������Ϊ������yarn.per-job-cluster.include-user-jar
���������ơ�

�������������ΪDISABLEDʱ��Flink�����û�·����jars������

user-jars��ϵͳ·��λ�ÿ���ͨ�����ò��������ƣ�
- ORDER��Ĭ�ϣ������ֵ�·��˳�����jar��ϵͳ��
- FIRST:ϵͳ·����ǰ����ӡ�
- LAST:ϵͳ·��������ӡ�

## Flink��YARN�ϵĻָ���Ϊ

Flink��YARN�ͻ������������ò�����������Ϊ������ʧ�ܺ���Щ������ͨ��conf/flink-conf.yaml���ã�Ҳ����ͨ��
������YARN�Ựʱ��-D�������á�

- `yarn.reallocate-failed`: ����Flink�Ƿ����·���ʧ�ܵ�Task Manager��Ĭ��true��
- `yarn.maximum-failed-containers`: ApplicationMaster���ܵ��������ʧ�ܸ�����ֱ��YARN�Ựʧ�ܡ�Ĭ����-n���õ�Task Manager������
- `yarn.application-attempts`: ApplicationMaster��+��ӵ�е�Task Manager�������ĳ��Դ�����Ĭ��1��ApplicationMasterʧ����YARN�Ự����ʧ�ܡ���YARN��ָ������ֵ�Ա�����ApplicationMaster��

## ����һ��ʧ�ܵ�YARN�Ự

�кܶ�ԭ��ʹ��һ��Flink��YARN�Ựʧ�ܡ�һ�������Hadoop��װ��HDFSȨ�ޣ�YARN���ã����汾���ݣ�����Flink��vanilla��Hadoop�ϣ�ȴ����Cloudera Hadoop��������ԭ��

### ��־�ļ�

����ʱFlink YARN�Ựʧ�ܣ��û���������Hadoop YARN����־��
�����õ���YARN��־���ϡ��û�������yarn-site.xml�ļ��а�yarn.log-aggregation-enable����ֵ����Ϊtrue��
ʹ����Ч��ֻҪ��һ����Ч���û�����ʹ����������������һ����ʧ�ܣ�yarn�Ự��������־�ļ���

~~~bash
yarn logs -applicationId <application ID>
~~~

�ڻỰ����ʱ��ȴ�������ֱ����־չʾ������

### YARN�ͻ��˿���̨&web�ӿ�

Flink YARN�ͻ���Ҳ�������ն����������Ϣ�����������ʱ������ĳʱ��Task Managerֹͣ������.���⣬��YARN��Դ��������web�ӿڣ�Ĭ����8088�˿ڣ��������Դ������web�ӿڵĶ˿���
yarn.resourcemanager.webapp.address����ֵ������

��webҳ��ɷ�������YARNӦ�ó������־�ļ�������ʾʧ��Ӧ�ó���������Ϣ��

## Ϊָ��Hadoop�汾����YARN�ͻ���

�û�ʹ����Hortonworks, Cloudera or MapR�ȹ�˾������Hadoop�����ǵ�Hadoop��HDFS���汾��YARN�汾�����빹��Flink��ͻ��
��ο�[��������](https://ci.apache.org/projects/flink/flink-docs-release-1.3/setup/building.html)��ø�ϸ���ܡ�

## ����ǽ����YARN����Flink

һЩYARN��Ⱥʹ�÷���ǽ�����Ƽ�Ⱥ����������֮������紫�䣬�����������£�Flink��job�ύ��YARN�Ự��ֻ��ͨ����Ⱥ���磨�ڷ���ǽ���󣩣�
��������������²����У�Flink��������һ����Χ�Ķ˿ڸ���ط���
����Щ��Χ�����£��û����Կ�Խ����ǽ�ύjob��Flink��

��ǰ��������������Ҫ�ύjob:

 * Job Manager��YARN�ϵ�ApplicationMaster��
 * ����Job Manager��BlobServer

���ύһ��job��Flink��BlobServer����ַ��û������е�jars�����й����ڵ㣨Task Manager����
Job Manager����job��������ִ�С�

�����������ò�����ָ���˿�:
 * `yarn.application-master.port`
 * `blob.server.port`

���������ÿɽ��յ����˿�ֵ����50010����Ҳ���Խ��շ�Χ��50000-50025��������
��ϣ�50010,50011,50020-50025,50050-50075��

��Hadoopʹ��ͬ���Ļ��ƣ����ò�����yarn.app.mapreduce.am.job.client.port-range��

## ����/�ڲ�

��С�ڼ�Ҫ����Flink��YARN��ν���.

<img src="{{ site.baseurl }}/fig/FlinkOnYarn.svg" class="img-responsive">

YARN�ͻ�����Ҫ����Hadoop������������YARN��Դ��������HDFS,�������Hadoop���ò�ȡ���²��ԣ�

* ����YARN_CONF_DIR, HADOOP_CONF_DIR or HADOOP_CONF_PATH ������˳���Ƿ������ã�����һ�������ˣ����ǾͿ��Զ�ȡ�����á�
* ������������ʧ�ܣ���ȷ��YARN��װ������ִ���������ͻ���ʹ��HADOOP_HOME�����������绷�����������ˣ��ͻ��˻᳢�Է���$HADOOP_HOME/etc/hadoop��hadoop2.*���� $HADOOP_HOME/conf��hadoop1.*��

������һ���µ�Flink YARN�Ự���ͻ��˻���ȷ���������Դ���������ڴ棩�Ƿ��ܻ�õ���
֮�󣬿ͻ����ϴ�����Flink��HDFS���õ�jars������1����

��һ���ͻ�������һ��YARN����������2��������ApplicationMaster������3����
�ͻ���ע�������ú�������Դ��jar�ļ���ָ���������е�YARN�ڵ��������׼���������������ļ�����
��Щ�����ˣ�ApplicationMaster (AM)�������ˡ�

Job Manager��AM������ͬһ����������ǳɹ�������AM֪��job����������ӵ�е��������ĵ�ַ��

Job ManagerΪTask Manager����һ���µ�Flink���ã�����task������Job Manager����

�ļ�Ҳ�ϴ���HDFS�ϡ�����AM����ҲΪFlink��web�ӿڷ���YARN��������ж˿��Ƿ������ʱ�˿ڡ�
������û�����ִ�ж��yarn�Ự��

Ȼ��AM�������䵽����������Щ������Flink��Task Manager����������jar�͸�������HDFS����
����Щ������ɺ�Flink�Ͱ�װ�����ˣ����Խ���job�ˡ�