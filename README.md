Zabbix Hadoop Monitor
=====================

+ What is it?

���̃c�[���́AZabbix�ɂ�Hadoop��HBase�̃��g���N�X�����W���邽�߂̃c�[���ł��BZabbix�̊O���X�N���v�g�Ƃ��ē��삵�܂��B�ȉ��̊��œ���m�F�����Ă��܂��B

* CDH3
* CDH4 + MRv1


+ ����ɕK�v�ȑO�����

* �Ď��ΏۂƂȂ�Hadoop�AHBase������ɓ��삵�Ă��邱�ƁB
* Zabbix�T�[�o���Z�b�g�A�b�v����A�O���X�N���v�g�ɂ������W���\�ɂȂ��Ă��邱�ƁB
* Zabbix�T�[�o��Perl��JSON���W���[�����C���X�g�[������Ă��邱�ƁB
** �X�N���v�g������Perl��JSON���W���[�����g�p���܂��B��������Ă��Ȃ��ꍇ�́A�K�X�C���X�g�[�����s���Ă��������B
    ��FCentOS�̃��|�W�g������C���X�g�[�����s���ꍇ
    $ sudo yum install perl-JSON


******

�{�c�[���̓���

******

* Hadoop and HBase setup

Hadoop�T�[�r�X����{�c�[�������g���N�X���擾�ł���悤���邽�߁A�ȉ��̐ݒ���s���܂��B

** CDH3�����ݒ�
CDH3�ɑ΂�������W���s���ꍇ�́AMetrics Servlet��L���ɂ��āA����������擾���ł���悤�ɂ���K�v������܂��B
���ł�Ganglia�ł̃��g���N�X���W���s���Ă���ꍇ�́AMetrics Servlet�������ɗL���ɂȂ��Ă���̂ŁA�ǉ��̐ݒ�͕K�v����܂���BMetrics Servlet�̂ݗL���ɂ���ꍇ�́A�ȉ��̒ʂ�̐ݒ���s���A�N���X�^�S�̂ɔz�z���A�Y����Hadoop�T�[�r�X�̍ċN�����s���Ă��������B

/etc/hadoop/conf/hadoop-metrics.properties

    dfs.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    dfs.period=30
    mapred.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    mapred.period=30
    jvm.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    jvm.period=30
    rpc.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    rpc.period=30
    ugi.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    ugi.period=30

/etc/hbase/conf/hadoop-metrics.properties
    hbase.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    hbase.period=30
    jvm.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    jvm.period=30
    rpc.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    rpc.period=30
    ugi.class=org.apache.hadoop.metrics.spi.NoEmitMetricsContext
    ugi.period=30


** TaskTracker�|�[�g�̌Œ�
TaskTracker����擾���郁�g���N�X�����Œ肷�邽�߁Amapred-site.xml�Ɉȉ��̐ݒ�������A�N���X�^�S�̂ɔz�z���܂��B�ݒ���I������ATaskTracker���ċN�����Ă��������B
    <property>
      <name>mapred.task.tracker.report.port</name>
      <value>50050</value>
    </property>

�����̐ݒ���s����|�͈ȉ��̃u���O�ɏ����Ă���̂ŁA��������������������B


** CDH4�����ݒ�
++ HDFS(Namenode, SecondaryNamenode, Datanode, Journalnode)
�����̃R���|�[�l���g�̓f�t�H���g��JMX Servlet�ɂ�郁�g���N�X�擾���ł���悤�ɂȂ��Ă��܂��B���̂��߁A���ɒǉ��̐ݒ�͕K�v����܂���B

++ ����ȊO�iJobTracker�ATaskTracker�AHBase Master�AHBase Regionserver�j
�����̃R���|�[�l���g�́AJMX Servlet�ɂ�郁�g���N�X�擾���ł��Ȃ����߁AMetrics Servlet��L���ɂ��āA����������擾���ł���悤�ɂ���K�v������܂��B�ݒ���@��CDH3�ł�Metrics Servlet�̐ݒ�Ɠ��l�ł��B

** TaskTracker�|�[�g�̌Œ�
CDH3�����ݒ�Ɠ��l�̐ݒ���s���܂��B�ݒ���I������TaskTracker���ċN�����Ă��������B


* �����W�X�N���v�g�̔z�u 

Zabbix�T�[�o�̊O���X�N���v�g�f�B���N�g���� "/etc/zabbix/externalscripts" �ƂȂ��Ă�����ł���΁A�ȉ��̃R�}���h�ɂăX�N���v�g�̔z�u���s�����Ƃ��ł��܂��B

 $ git clone zbx_hadoop_monitor
 $ cd zbx_hadoop_monitor
 $ ./install.sh

��L�ȊO��Zabbix�T�[�o�̊O���X�N���v�g�f�B���N�g���Ƃ��Ă���ꍇ�́A�蓮�ňȉ��̂悤�Ƀt�@�C����z�u���Ă��������B


* Zabbix�e���v���[�g�̃C���|�[�g

�{�T�C�g��template�f�B���N�g���ɔz�u����Ă���XML�t�@�C����Zabbix���C���|�[�g���Ă��������B
�i�z�z���Ă���e���v���[�g�́A�W���I�ȍ\���݂̂ɑΉ��������̂ɂȂ�܂��B���̒��Ɋ܂܂�Ȃ����g���N�X���Ď�����ꍇ�́A�Y�t�c�[���ɂăe���v���[�g�𐶐�����K�v������܂��j


+ �Ď��Ώۂւ̃e���v���[�g�K�p

�O���ŃC���|�[�g�����e���v���[�g��Ή�����T�[�r�X�����삵�Ă���T�[�o�ɓK�p���Ă��������B�����W���J�n����܂��B


******

���̃c�[���̓���d�l�ɂ���

******

+ ����T�v
���̃c�[���́AZabbix�̊O���X�N���v�g�A�C�e���ɂď����W�X�N���v�g���N�����A�eHadoop�T�[�r�X����JSON�`���ŏ����擾���A����𐮌`����zabbix_sender���g���āA���g���N�X���Ƃ̊Ď��A�C�e���ɓo�^���܂��B
�O���X�N���v�g���N������Ď��A�C�e���ɂ́A�X�N���v�g�̎��s���ʂ��o�^����܂��B�ʏ펞�͂�����zabbix_sender�̖߂�l���o�^����܂��B�e���v���[�g�ɓo�^����Ă��Ȃ��A�C�e��������ꍇ�́A���̃��O����m�F���邱�Ƃ��ł��܂��B�܂��A�X�N���v�g�����삵���̂��ɁA���擾�Ɏ��s���Ă���ꍇ�̓G���[���b�Z�[�W���o�^����܂��B


* �X�N���v�g�I�v�V����
���̃c�[���ɂ͈ȉ��̃I�v�V����������܂��B�i���ɖ��L����Ă��Ȃ����̂́A�X�N���v�g���ʂ̃I�v�V�����ł��j

** --detailed
detailed metrics
���\�b�h���Ƃ̎��s���ԂȂǂ̏ڍׂȃf�[�^���擾���邱�Ƃ��ł��܂����A�擾�f�[�^�ʂ������Ȃ��Ă��܂����߁A�f�t�H���g�ł͎擾���Ȃ��悤�ɂȂ��Ă��܂��B
���̃��g���N�X���g���ꍇ�́A�X�N���v�g���s����"--detailed"�Ƃ����I�v�V���������Ă��������B

** --nosend

** --javalang (get_hadoop_jmx.pl only)

JMX Servlet �o�R�Ŏ擾�ł�����ɂ́AHadoop�T�[�r�X�ŗL�̂��̂̂ق��ɁA��ʓI��JMX�̒l���܂߂܂�Ă��܂��B���̂����A"java.lang"�ɕ��ނ���Ă���q�[�v�g�p�ʓ���ǉ��擾���邽�߂̃I�v�V�����ł��BHadoop�T�[�r�X�ŗL�̃��g���N�X�Əd�Ȃ��Ă��镔�����������߁A�f�t�H���g�ł̓I�t�ɂȂ��Ă��܂��B

** --dump_json

�X�N���v�g�f�o�b�O�p�̃I�v�V�����ł��B�W���G���[�o�͂�JSON�f�[�^��Dump���o�͂��܂��B


* Zabbix Proxy�\���ւ̑Ή�
���̃c�[����Zabbix Proxy�\���ł����삵�܂��B���̏ꍇ�́A�����ŏЉ�Ă���C���X�g�[���菇�ɏ����āAZabbix Proxy�̊O���X�N���v�g�f�B���N�g���ɃX�N���v�g��z�u���Ă��������B


******

�t���c�[�����g����Zabbix�e���v���[�g�̐���

******

* �e���v���[�g�����c�[���̊�{�I�Ȏg����

�{�c�[���ɂ́A�����W�X�N���v�g��Hadoop�T�[�r�X����擾�����f�[�^�����Ƃ�Zabbix�e���v���[�g�𐶐�����X�N���v�g���t�����Ă��܂��B�����W�X�N���v�g�ɂ͂��̃c�[���ƘA�g���邽�߁AZabbix�Ƀf�[�^�𑗐M�����A�W���o�͂Ɏ擾���ʂ��o�͂���I�v�V����������A�ȉ��̂悤�Ɏg�p���܂��B

 $ ./get_hadoop_metrics.pl dummy hostname port --nosend | ./convert_infile.pl > template_name.xml

���Ƃ��΁AQJM���g����Namenode HA�\���ł́AJournalnode�ւ̏������ݒx���Ɋւ��郁�g���N�X���擾�\�ł��B�������A���̃��g���N�X����Journalnode��IP�A�h���X�ƃ|�[�g�ԍ��Ɉˑ����邽�߁A���̊��Ńe���v���[�g�̐������s���K�v������܂��B


* �O���t�A�g���K�[�ݒ���܂߂���������

�擾�Ώۂɍ��킹���R���t�B�O�t�@�C�����쐬���邱�ƂŁA�O���t��g���K�[���܂񂾃e���v���[�g�𐶐����邱�Ƃ��ł��܂��B�g�����͐\����Ȃ��ł����A�\�[�X�R�[�h���Q�Ƃ��Ă��������B


******

���̂ق�

******

���̃c�[����Perl���g���Ă��܂����APython�ŏ������������Ǝv���Ă��܂��B���g���N�X�ɂ��ẮAASF�ł��h�L�������g����������Ă��Ȃ��̂ŁA�擾���e�͓K�X�m�F���Ă��������B
