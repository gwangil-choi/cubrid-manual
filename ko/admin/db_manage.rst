
:meta-keywords: cubrid databases.txt, cubrid users, cubrid volume
:meta-description: How to manage CUBRID Databases, Users and Volumes.

.. role:: red

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
 
    *   :red:`영구적 데이터를 저장하지만 일시적 데이터도 저장할 수 있는 데이터 볼륨이 있다.`
    *   :red:`활성 로그, 보관 로그 및 백그라운드 보관 로그로 세분화할 수 있는 로그 볼륨이 있다.`
    
*   :red:`일시적 볼륨에는 일시적 데이터만 저장된다.`

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

    *  :red:`위의 표에서 *testdb*, *testdb_perm*, *testdb_temp*, *testdb_x003* ~ *testdb_x006*은 데이터베이스 볼륨 파일로 분류된다.`
    *  :red:`파일 크기는 **cubrid createdb** 및 **cubrid addvoldb**의 **--db-volume-size** 옵션과 **cubrid.conf** 의  **db_volume_size**에 의해 결정된다.`
    *  :red:`데이터베이스에 공간이 부족해지면 자동으로 새 볼륨을 생성한다.`

*   일시적 볼륨 

    *  :red:`일시적 볼륨은 일반적으로 일시적 데이터를 저장하는 데 사용된다. 이 볼륨은 데이터베이스별로 자동으로 생성되고 삭제된다.`
    *  :red:`파일 크기는 **cubrid.conf**의 **db_volume_size**에 의해 결정된다.`

*   로그 볼륨 파일

    *   :red:`위의 표에서 *testdb_lgar_t*, *testdb_lgar224* 및 *testdb_lgat*는 로그 볼륨 파일로 분류된다.`
    *   :red:`파일 크기는 **cubrid.conf**의 **log_volume_size** 또는 **cubrid createdb**의 **--log-volume-size** 옵션에 의해 결정된다.`

.. note::

    :red:`데이터베이스 재시작과 비정상 종료 시에도 보존해야 하는 데이터는 영구적 데이터 용도로 생성된 데이터베이스 볼륨에 저장된다. 이 볼륨은 테이블 행(힙 파일), 인덱스(b-tree 파일) 및 여러 시스템 파일을 저장한다.`

    :red:`질의 처리 및 정렬의 중간 결과와 최종 결과의 경우 일시적 저장소만 필요하다. 필요한 일시적 데이터 크기에 따라 우선 메모리에 저장된다(공간 크기는 **cubrid.conf**에 지정된 시스템 파라미터 **temp_file_memory_size_in_pages**에 의해 결정됨). 이를 초과하는 데이터는 디스크에 저장한다.`

    :red:`데이터베이스에서는 일시적 데이터를 위한 디스크 공간을 할당하기 위해 일반적으로 일시적 볼륨을 생성해 사용한다. 그러나 관리자는 **cubrid addvoldb -p temp** 명령을 사용해 일시적 데이터를 저장하기 위한 용도로 영구적 데이터베이스 볼륨을 할당할 수 있다. 이러한 영구적 데이터베이스 볼륨 이 있는 경우 임시 데이터를 디스크 공간에 저장할 때  일시적 볼륨 보다 우선 사용한다.`

    :red:`일시적 데이터를 사용할 수 있는 질의의 예는 다음과 같다.`

    *   **SELECT** :red:`등의 결과 집합이 생성되는 질의`
    *   **GROUP BY** 나 **ORDER BY** 가 포함된 질의
    *   부질의(subquery)가 포함된 질의
    *   정렬 병합(sort-merge) 조인이 수행되는 질의
    *   **CREATE INDEX** 질의문이 포함된 질의

    :red:`일시적 데이터에 의해 시스템의 디스크 공간이 모두 사용되는 것을 방지하려면 다음을 수행할 것을 권장한다.`

       *   :red:`영구적 데이터베이스 볼륨을 미리 생성해 일시적 데이터에 필요한 공간을 확보한다.`
       *   :red:`**cubrid.conf**에서 **temp_file_max_size_in_pages** 파라미터를 설정해 질의를 수행할 때 일시적 볼륨에 사용되는 공간의 크기를 제한한다(기본적으로는 제한 없음).`

