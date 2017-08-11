
:meta-keywords: kill statement
:meta-description: The KILL statement terminates transactions with an option TRANSACTION or QUERY modifier.

****
KILL
****

**KILL** 명령은 트랜잭션들을 종료시키는데 변형자 **TRANSACTION** 또는 **QUERY** 옵션과 함께 사용된다.

::

    KILL [TRANSACTION | QUERY] 트랜 인덱스, ... ;

\

* **KILL TRANSACTION** 이 **KILL** 문장의 기본이다. 이는 변형자 없이 **KILL** 명령을 실행하는 것과 같다. 이것은 주어진 *트랜 인덱스* 와 연관된 연결을 종료시킨다. 
* **KILL QUERY** 는 실행중인 해당 트랜잭션을 종료시킨다. 

DBA와 DBA 그룹에 포함되는 사용자는 시스템의 모든 트랜잭션들을 종료시킬 수 있다, 반면 DBA가 아닌 사용자들은 자신의 트랜잭션 만을 종료시킬 수 있디.    

::

    KILL TRANSACTION 1;
    
