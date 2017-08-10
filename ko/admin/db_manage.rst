
:meta-keywords: cubrid databases.txt, cubrid users, cubrid volume
:meta-description: How to manage CUBRID Databases, Users and Volumes.

데이터베이스 관리
=================

데이터베이스 사용자
-------------------

CUBRID 데이터베이스 사용자는 동일한 권한을 갖는 멤버를 가질 수 있다. 사용자에게 권한 **A**\가 부여되면, 상기 사용자에게 속하는 모든 멤버에게도 권한 **A**\가 동일하게 부여된다. 이와 같이 데이터베이스 사용자와 그에 속한 멤버를 '그룹'이라 하고, 멤버가 없는 사용자를 '사용자'라 한다.

CUBRID는 **DBA**\와 **PUBLIC**\이라는 사용자를 기본으로 제공한다.

*   **DBA**\는 모든 사용자의 멤버가 되며 데이터베이스의 모든 객체에 접근할 수 있는 최고 권한 사용자이다. 또한, **DBA**\만이 데이터베이스 사용자를 추가, 편집, 삭제할 수 있는 권한을 갖는다.

*   **DBA**\를 포함한 모든 사용자는 **PUBLIC**\ 의 멤버가 되므로 모든 데이터베이스 사용자는 **PUBLIC**\에 부여된 권한을 가진다. 예를 들어, **PUBLIC** 사용자에 권한 **B**\를 추가하면 데이터베이스의 모든 사용자에게 일괄적으로 권한 **B**\가 부여된다.  사용자의 추가, 권한 부여에 대한 자세한 내용은 :doc:`/sql/authorization`\ 을 참고한다.

.. _databases-txt-file:

databases.txt 파일
------------------

CUBRID에 존재하는 모든 데이터베이스의 위치 정보는 **databases.txt** 파일에 저장하는데, 이를 데이터베이스 위치 정보 파일이라 한다. 이러한 데이터베이스 위치 정보 파일은 데이터베이스의 생성, 이름 변경, 삭제 및 복사에 관한 유틸리티를 수행하거나 각 데이터베이스를 구동할 때에 사용되며, 기본으로는 설치 디렉터리의 **databases** 디렉터리에 위치하고, **CUBRID_DATABASES** 환경 변수로 디렉터리 위치를 지정할 수 있다.

::

    db_name db_directory server_host logfile_directory

데이터베이스 위치 정보 파일의 라인별 형식은 구문에 정의된 바와 같으며, 데이터베이스 이름, 데이터베이스 경로, 서버 호스트 및 로그 파일의 경로에 관한 정보를 저장한다. 다음은 데이터베이스 위치 정보 파일의 내용을 확인한 예이다.

::

    % more databases.txt
    
    dist_testdb /home1/user/CUBRID/bin d85007 /home1/user/CUBRID/bin
    dist_demodb /home1/user/CUBRID/bin d85007 /home1/user/CUBRID/bin
    testdb /home1/user/CUBRID/databases/testdb d85007 /home1/user/CUBRID/databases/testdb
    demodb /home1/user/CUBRID/databases/demodb d85007 /home1/user/CUBRID/databases/demodb

데이터베이스 위치 정보 파일의 저장 디렉터리는 기본적으로 설치 디렉터리의 **databases** 디렉터리로 지정되며, 시스템 환경 변수 **CUBRID_DATABASES**\ 의 설정을 변경하여 기본 디렉터리를 변경할 수 있다. 데이터베이스 위치 정보 파일의 저장 디렉터리 경로가 유효해야 데이터베이스 관리를 위한 **cubrid** 유틸리티가 데이터베이스 위치 정보 파일에 접근할 수 있게 된다. 이를 위해서 사용자는 디렉터리 경로를 정확하게 입력해야 하고, 해당 디렉터리 경로에 대해 쓰기 권한을 가지는지 확인해야 한다. 다음은 **CUBRID_DATABASES** 환경 변수에 설정된 값을 확인하는 예이다.

::

    % set | grep CUBRID_DATABASES
    CUBRID_DATABASES=/home1/user/CUBRID/databases

만약 **CUBRID_DATABASES** 환경 변수에서 유효하지 않은 디렉터리 경로가 설정되는 경우에는 에러가 발생하며, 설정된 디렉터리 경로는 유효하나 데이터베이스 위치 정보 파일이 존재하지 않는 경우에는 새로운 위치 정보 파일을 생성한다. 또한, **CUBRID_DATABASES** 환경 변수가 아예 설정되지 않은 경우에는 현재 작업 디렉터리에서 위치 정보 파일을 검색한다.

.. _database-volume:

데이터베이스 볼륨
-----------------

CUBRID 데이터베이스의 볼륨은 크게 영구적 볼륨, 일시적 볼륨, 백업 볼륨으로 분류한다. 

*   영구적 볼륨 중
 
    *   there are data volumes, that usually store permanent data, but can also store temporary data.
    *   there are log volumes, that can be further classified as: one active log, archive logs and one background archiving log.
    
*   In the temporary volumes, only temporary data is stored.

볼륨에 대한 자세한 내용은 :ref:`database-volume-structure`\ 를 참고한다.

다음은 *testdb* 데이터베이스를 운영할 때 발생하는 데이터베이스 관련 파일의 예이다.

+----------------+-------+-----------------+----------------+------------------------------------------------------------------------------------------------------+
| File name      | Size  | Purpose         | Classification | Description                                                                                          |
+================+=======+=================+================+======================================================================================================+
| testdb         | 512MB | | permanent     | | Database     | | The firstly created volume when DB is created.                                                     |
|                |       | | data          | | volume       | | This volume stores permanent data (system, heap and index files).                                  |
|                |       |                 |                | | This volume includes database meta information.                                                    |
|                |       |                 |                | | **cubrid createdb** uses by default the size specified by **db_volume_size** in **cubrid.conf**.   |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_perm    | 512MB | | permanent     |                | | Manually added volume using **cubrid addvoldb** utility                                            |
|                |       | | data          |                | | This volume stores permanent data (system, heap and index files).                                  |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_temp    | 512MB | | temporary     |                | | Manually added volume using **cubrid addvoldb** utility                                            |
|                |       | | data          |                | | This volume stores temporary data (query results, list files, sort files, join object hashes).     |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_x003    | 512MB | | permanent     |                | | Automatically created when database requires more space.                                           |
|                |       | | data          |                | | This volume stores permanent data (system, heap and index files).                                  |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_x004    | 512MB | | permanent     |                | | Automatically created when database requires more space.                                           |
|                |       | | data          |                | | This volume stores permanent data (system, heap and index files).                                  |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_x005    | 512MB | | permanent     |                | | Automatically created when database requires more space.                                           |
|                |       | | data          |                | | This volume stores permanent data (system, heap and index files).                                  |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_x006    | 64MB  | | permanent     |                | | Automatically created when database requires more space.                                           |
|                |       | | data          |                | | This volume stores permanent data (system, heap and index files).                                  |
|                |       |                 |                | | The size of volume is not maximized (yet).                                                         |
+----------------+-------+-----------------+----------------+------------------------------------------------------------------------------------------------------+
| testdb_t32766  | 512MB | | temporary     | | Temporary    | | Automatically created when database requires more space.                                           |
|                |       | | data          | | Volume       | | This volume stores temporary data (query results, list files, sort files, join object hashes).     |
+----------------+-------+-----------------+----------------+------------------------------------------------------------------------------------------------------+
| testdb_lgar_t  | 512MB | | background    | | Log          | | A log file which is related to the background archiving feature.                                   |
|                |       | | archiving     | | volume       | | This is used when storing the archiving log.                                                       |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_lgar224 | 512MB | | archive       |                | | Archiving logs are continuously archived and the files ending with three digits are created.       |
|                |       |                 |                | | At this time, archiving logs from 001~223 seem to be removed normally by **cubrid backupdb** -r    |
|                |       |                 |                | | option or the setting of **log_max_archives** in **cubrid.conf**. When archiving logs are removed, |
|                |       |                 |                | | you can see the removed archiving log numbers in the REMOVE section of lginf file.                 |
|                |       |                 |                | | See :ref:`managing-archive-logs`.                                                                  |
+----------------+-------+-----------------+                +------------------------------------------------------------------------------------------------------+
| testdb_lgat    | 512MB | | active        |                | | Active log file                                                                                    |
+----------------+-------+-----------------+----------------+------------------------------------------------------------------------------------------------------+

*   데이터베이스 볼륨 파일

    *  In the table above, *testdb*, *testdb_perm*, *testdb_temp*, *testdb_x003* ~ *testdb_x006* are classified as the database volume files.
    *  File size is determined by **db_volume_size** in **cubrid.conf** or the **--db-volume-size** option of **cubrid createdb** and **cubrid addvoldb**.
    *  When database remains out of space, it automatically expands existing volumes and creates new volumes.

*   Temporary volume

    *  Temporary volumes are usually used to store temporary data. They are automatically created and destroyed by database.
    *  File size is determined by **db_volume_size** in **cubrid.conf**.

*   로그 볼륨 파일

    *   위의 예에서 *testdb_lgar_t*, *testdb_lgar224*, *testdb_lgat* 가 로그 볼륨 파일에 해당된다.
    *   File size is determined by **log_volume_size** in **cubrid.conf** or the **--log-volume-size** option of **cubrid createdb**.

.. note::

    Any data that has to be persistent over database restart and crash is stored in the database volumes created for permanent data purpose. The volumes store table rows (heap files), indexes (b-tree files) and several system files.

    Intermediate and final results of query processing and sorting need only temporary storage. Based on the size of required temporary data, it will be first stored in memory (the space size is determined by the system parameter **temp_file_memory_size_in_pages** specified in **cubrid.conf**). Exceeding data has to be stored on disk.

    Database will usually create and use temporary volumes to allocate disk space for temporary data. Administrator may however assign permanent database volumes with the purpose of storing temporary data using by running **cubrid addvoldb -p temp** command. If such volumes exist, they will have priority over temporary volumes when disk space is allocated for temporary data.

    The examples of queries that can use temporary data are as follows:

    *   **SELECT** 문 등 질의 결과가 생성되는 질의
    *   **GROUP BY** 나 **ORDER BY** 가 포함된 질의
    *   부질의(subquery)가 포함된 질의
    *   정렬 병합(sort-merge) 조인이 수행되는 질의
    *   **CREATE INDEX** 문이 포함된 질의

    To have complete control on the disk space used for temporary data and to prevent it from consuming all system disk space, our recommendation is to:

       *   create permanent database volumes in advance to secure the required space for temporary data
       *   limit the size of the space used in the temporary volumes when a queries are executed by setting **temp_file_max_size_in_pages** parameter in **cubrid.conf** (there is no limit by default).

