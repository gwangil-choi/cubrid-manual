*************
ODBC 드라이버
*************

**개요**

**설명**

CUBRID ODBC 드라이버는 ODBC 3.52 버전을 지원하며, ODBC 코어 부분과 Level 1과 Level 2 일부 API를 지원한다. CUBRID ODBC 드라이버는 ODBC Spec 3.x를 기반으로 구현되었으므로 ODBC Spec 2.x을 이용하여 작성한 프로그램에 대해서 하위 호환성을 완벽하게 보장하지는 않는다. 32비트만 지원하므로, Windows 64비트 환경이라도 CUBRID 32비트 버전을 설치해야 한다. Windows 64비트 환경에서 CUBRID 32비트의 ODBC 드라이버는
**C:\Windows\SysWOW64\odbcad32.exe**
를 실행하여 확인할 수 있다.

CUBRID ODBC 드라이버는 CCI API를 기반으로 작성되었으므로, CCI API 및 CCI에 적용되는
**CCI_DEFAULT_AUTOCOMMIT**
과 같은 설정 파라미터에 영향을 받는다.

별도로 ODBC 드라이버를 다운로드하거나 ODBC 드라이버에 대한 최신 정보를 확인하려면
`http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver>`_
에 접속한다.

**CUBRID와**
**ODBC의**
**데이터**
**타입**
**매핑**

다음은 CUBRID가 지원하는 데이터 타입과 ODBC 데이터 타입을 매핑한 표이다.

+-------------------------+--------------------------------+
| **CUBRID 데이터 타입**  | **ODBC 데이터 타입**           |
|                         |                                |
+-------------------------+--------------------------------+
| Char                    | SQL_CHAR                       |
|                         |                                |
+-------------------------+--------------------------------+
| Varchar                 | SQL_VARCHAR                    |
|                         |                                |
+-------------------------+--------------------------------+
| String                  | SQL_LONGVARCHAR                |
|                         |                                |
+-------------------------+--------------------------------+
| Nchar                   | SQL_CHAR                       |
|                         |                                |
+-------------------------+--------------------------------+
| Varnchar                | SQL_VARCHAR                    |
|                         |                                |
+-------------------------+--------------------------------+
| Bit                     | SQL_BINARY                     |
|                         |                                |
+-------------------------+--------------------------------+
| varying bit             | SQL_VARBINARY                  |
|                         |                                |
+-------------------------+--------------------------------+
| Numeric                 | SQL_NUMERIC                    |
|                         |                                |
+-------------------------+--------------------------------+
| Int                     | SQL_INTEGER                    |
|                         |                                |
+-------------------------+--------------------------------+
| Short                   | SQL_SMALLINT                   |
|                         |                                |
+-------------------------+--------------------------------+
| Float                   | SQL_FLOAT                      |
|                         |                                |
+-------------------------+--------------------------------+
| Double                  | SQL_DOUBLE                     |
|                         |                                |
+-------------------------+--------------------------------+
| Bigint                  | SQL_BIGINT                     |
|                         |                                |
+-------------------------+--------------------------------+
| Date                    | SQL_TYPE_DATE                  |
|                         |                                |
+-------------------------+--------------------------------+
| Time                    | SQL_TYPE_TIME                  |
|                         |                                |
+-------------------------+--------------------------------+
| Timestamp               | SQL_TYPE_TIMESTAMP             |
|                         |                                |
+-------------------------+--------------------------------+
| Datetime                | SQL_TYPE_TIMESTAMP             |
|                         |                                |
+-------------------------+--------------------------------+
| Monetary                | SQL_DOUBLE                     |
|                         |                                |
+-------------------------+--------------------------------+
| Oid                     | SQL_CHAR(32)                   |
|                         |                                |
+-------------------------+--------------------------------+
| set, multiset, sequence | SQL_VARCHAR(MAX_STRING_LENGTH) |
|                         |                                |
+-------------------------+--------------------------------+

**ODBC와**
**ASP**
**설치**
**및**
**설정**

**기본**
**환경**

*   CUBRID 2008 R3.1(8.3.1) 이상, 32비트 버전



**ODBC**
**드라이버**
**설정**

CUBRID ODBC Driver는 CUBRID 설치 시 자동으로 설치되며, [제어판] > [관리 도구] > [데이터 원본(ODBC)]을 실행하면 [드라이버] 탭에서 확인할 수 있다.

|image77_gif|

**DNS**
**설정**

CUBRID ODBC Driver가 확인되었다면 응용 프로그램에서 접속하려는 데이터베이스로 DSN을 설정한다. DSN 설정을 위해 ODBC 데이터 원본 관리자에서 [추가] 버튼을 클릭하면 다음과 같은 대화 상자가 나타난다.
**CUBRID Driver**
를 선택하고 [마침] 버튼을 클릭한다.

|image78_gif|

[Config CUBRID Data Sources] 대화 상자가 나타나면 다음과 같은 내용을 입력한다.

|image79_png|

*   **DSN**
    : 데이터 원본의 이름을 설정한다.



*   **DB Name**
    : 접속할 데이터베이스 이름을 입력한다.



*   **DB User**
    : 데이터베이스 사용자 이름을 입력한다.



*   **Password**
    : 데이터베이스 사용자 비밀번호를 입력한다.



*   **Server Address**
    : 데이터베이스의 호스트 주소를 입력한다.
    **localhost**
    또는 다른 서버의 IP 주소를 입력한다.



*   **Server Port**
    : CUBRID 브로커의 포트 번호를 입력한다. 기본값은 33000이다. CUBRID 브로커 포트 번호를 확인하려면
    **cubrid_broker.conf**
    파일에서 BROKER_PORT 값을 확인하거나, 터미널에서 cubrid service status 명령을 입력한다. 터미널에서 입력했을 때 화면은 다음과 같다.



|image80_png|

*   **FETCH_SIZE**
    : ODBC 드라이버가 내부적으로 사용하는 CCI 라이브러리의
    **cci_fetch**
    () 함수를 호출할 때마다 서버로부터 fetch하는 레코드의 개수를 설정한다.



위와 같이 입력한 후 [확인]을 클릭하면 다음과 같이 [User Data Sources]에 데이터 원본이 추가된 것을 확인할 수 있다.

|image81_png|

**DSN을**
**사용하지**
**않고**
**데이터베이스에**
**직접**
**연결**

연결 문자열을 사용하여 CUBRID 데이터베이스에 직접 연결할 수도 있다. 연결 문자열의 예는 다음과 같다. 자세한 내용은
`연결 문자열(connection string) 구성 <#api_api_odbc_programming_htm_con_5232>`_
을 참고한다.

conn = "driver={CUBRID Driver};server=localhost;port=33000;uid=dba;pwd=;db_name=demodb;"

**참고**
CUBRID 데이터베이스에 연결하기 전에 데이터베이스를 먼저 시작하지 않으면 ODBC 오류가 발생한다. 데이터베이스(demodb)를 시작하려면 터미널에
**cubrid server start demodb**
명령을 입력한다.

**참고**
ODBC 드라이버에 대한 최신 정보를 확인하려면
`http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver>`_
에 접속한다.

**ODBC**
**프로그래밍**

**연결**
**문자열(connection string)**
**구성**

CUBRID ODBC 프로그래밍을 할 때 연결 문자열(connection string)은 다음과 같이 작성한다.

+-------------+-------------+-------------------------------+
| **항목**      | **예**       | **설명**                        |
|             |             |                               |
+-------------+-------------+-------------------------------+
| Driver      | CUBRID      | 드라이버 이름                       |
|             | ?Driver     |                               |
|             |             |                               |
+-------------+-------------+-------------------------------+
| UID         | PUBLIC      | 사용자 아이디                       |
|             |             |                               |
+-------------+-------------+-------------------------------+
| PWD         | xxx         | 비밀번호                          |
|             |             |                               |
+-------------+-------------+-------------------------------+
| FETCH_SIZE  | 100         | Fetch 크기                      |
|             |             |                               |
+-------------+-------------+-------------------------------+
| PORT        | 33000       | 브로커 포트?번호                     |
|             |             |                               |
+-------------+-------------+-------------------------------+
| SERVER      | 127.0.0.1   | CUBRID 브로커 서버 IP 주소 또는 호스트 이름 |
|             |             |                               |
+-------------+-------------+-------------------------------+
| DB_NAME     | demodb      | 데이터베이스 이름                     |
|             |             |                               |
+-------------+-------------+-------------------------------+
| DESCRIPTION | cubrid_test | 설명                            |
|             |             |                               |
+-------------+-------------+-------------------------------+
| CHARSET     | utf-8       | 문자셋                           |
|             |             |                               |
+-------------+-------------+-------------------------------+

위의 예를 이용한 연결 문자열은 다음과 같다.

"DRIVER=CUBRID Driver;UID=PUBLIC;PWD=xxx;FETCH_SIZE=100;PORT=33000;SERVER=127.0.0.1;DB_NAME=demodb;DESCRIPTION=cubrid_test;CHARSET=utf-8"

**주의**
**사항**

*   연결 문자열에서 세미콜론(;)은 구분자로 사용되므로, 연결 문자열에 암호(PWD)를 지정할 때 암호의 일부에 세미콜론을 사용할 수 없다.



*   스레드 기반 프로그램에서 데이터베이스 연결은 각 스레드마다 독립적으로 사용해야 한다.



**OID와**
**컬렉션**
**사용**

ODBC는 관계형 DBMS에 맞게 설계되었다. 그러므로 CUBRID ODBC 드라이버는 CUBRID의 OID와 컬렉션과 같은 일부 객체지향 특징을 지원하지 않는다. CUBRID는 관계형 데이터 모델과 객체지향 데이터 모델을 통합한 객체관계형 DBMS이기 때문이다.

**OID**
**사용**

CUBRID ODBC 드라이버는 OID를 string (char(32))으로 간주하므로 OID를 포함하는
**INSERT**
,
**UPDATE**
,
**DELETE**
를 아래와 같이 사용할 수 있다. OID string을 사용할 때는 반드시 작은 따옴표 ('')처리가 필요하다. 다음 예제의 member 속성의 도메인은 객체(OID)와 같다.

insert into foo(member) values('@12|34|56')
delete from foo where member = '@12|34|56'
update foo set age = age + 1 where member = '@12|34|56'

**컬렉션**
**사용**

컬렉션 타입 :
**SET**
,
**MULTISET**
,
**SEQUENCE**
세가지가 있다. CUBRID ODBC 드라이버는 컬렉션을 string(longvarchar)으로 간주한다.?컬렉션은
**SELECT**
질의에서 "{value_1, value_2, ... value_n}"와 같이 중괄호 안의 쉼표를 이용해 각 요소를 구분하여 획득한다.

**참고**
칼럼에서 정의한 크기보다 큰 문자열을
**INSERT**
/
**UPDATE**
하면 문자열이 잘려서 입력된다.

**참고**
ODBC 드라이버에 대한 최신 정보를 확인하려면
`http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver>`_
에 접속한다.

**ASP**
**예제**
**프로그램**

ASP 예제를 실행할 가상 디렉터리의 '기본 웹 사이트' 항목에서 마우스 오른쪽 버튼을 클릭한 뒤 [속성]을 클릭한다.
|image82_png|
?

위의 그림에서
**웹사이트 확인**
아래
**IP 주소**
를
**(모두 할당되지 않음)**
으로 선택하면 localhost로 인식한다. 특정한 IP 주소를 통해 예제를 확인하려면 해당 IP에 해당 디렉터리를 가상 디렉터리로 인식시키고 등록 정보에 IP 주소를 등록한다.

다음은 IP 주소를 localhost로 설정한 예제이다.

**예제**

아래의 예제 코드를 cubrid.asp로 만들고 가상 디렉터리에 저장한다.

<HTML>
??? <HEAD>
?????<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
?????? <title>CUBRID Query Test Page</title>
??</HEAD>
?<BODY topmargin="0" leftmargin="0">
???
?<table border="0" width="748" cellspacing="0" cellpadding="0">
?? <tr>
???? <td width="200"></td>
???? <td width="287">
?????? <p align="center"><font size="3" face="Times New Roman"><b><font color="#FF0000">CUBRID</font>Query Test</b></font></td>
???? <td width="200"></td>
?? </tr>
?</table>
?<form action="cubrid.asp" method="post" >
?<table border="1" width="700" cellspacing="0" cellpadding="0" height="45">
?? <tr>
???? <td width="113" valign="bottom" height="16" bgcolor="#DBD7BD" bordercolorlight="#FFFFCC"><font size="2">SERVER IP</font></td>
???? <td width="78"? valign="bottom" height="16" bgcolor="#DBD7BD" bordercolorlight="#FFFFCC"><font size="2">Broker PORT</font></td>
???? <td width="148" valign="bottom" height="16" bgcolor="#DBD7BD" bordercolorlight="#FFFFCC"><font size="2">DB NAME</font></td>
???? <td width="113" valign="bottom" height="16" bgcolor="#DBD7BD" bordercolorlight="#FFFFCC"><font size="2">DB USER</font></td>
???? <td width="113" valign="bottom" height="16" bgcolor="#DBD7BD" bordercolorlight="#FFFFCC"><font size="2">DB PASS</font></td>
???? <td width="80" height="37" rowspan="4" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED">　
?????? <p><input type="submit" value="실행하기" name="B1" tabindex="7"></p></td>
?? </tr>
?? <tr>
???? <td width="113" height="1" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><font size="2"><input type="text" name="server_ip" size="20" tabindex="1" maxlength="15" value="<%=Request("server_ip")%>"></font></td>
???? <td width="78"? height="1" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><font size="2"><input type="text" name="cas_port" size="15" tabindex="2" maxlength="6" value="<%=Request("cas_port")%>"></font></td>
???? <td width="148" height="1" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><font size="2"><input type="text" name="db_name" size="20" tabindex="3" maxlength="20" value="<%=Request("db_name")%>"></font></td>
???? <td width="113" height="1" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><font size="2"><input type="text" name="db_user" size="15" tabindex="4" value="<%=Request("db_user")%>"></font></td>
???? <td width="113" height="1" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><font size="2"><input type="password" name="db_pass" size="15" tabindex="5" value="<%=Request("db_pass")%>"></font></td>
?? </tr>
?? <tr>
???? <td width="573" colspan="5" valign="bottom" height="18" bordercolorlight="#FFFFCC" bgcolor="#DBD7BD"><font size="2">QUERY</font></td>
?? </tr>
?? <tr>
???? <td width="573" colspan="5" height="25" bordercolorlight="#FFFFCC" bgcolor="#F5F5ED"><textarea rows="3" name="query" cols="92" tabindex="6"><%=Request("query")%></textarea></td>
?? </tr>
?</table>
?</form>
?<hr>
</BODY>
</HTML>
<%
??? ' DSN과
SQL문을
가져온다.
??? strIP = Request( "server_ip" )
??? strPort = Request( "cas_port" )
??? strUser = Request( "db_user" )
??? strPass = Request( "db_pass" )
??? strName = Request( "db_name" )
??? strQuery = Request( "query" )
???
if strIP = "" then
?? Response.Write "SERVER_IP를
입력하세요"
??????? Response.End ' IP가
없으면
페이지
종료
??? end if
??? if strPort = "" then
??? ?? Response.Write "Port
번호를
입력하세요"
??????? Response.End ' Port가
없으면
페이지
종료
??? end if
??? if strUser = "" then
??? ?? Response.Write "DB_USER를
입력하세요"
??????? Response.End ' DB_User가
없으면
페이지
종료
??? end if
??? if strName = "" then
??? ?? Response.Write "DB_NAME을
입력하세요"
??????? Response.End ' DB_NAME이
없으면
페이지
종료
??? end if
??? if strQuery = "" then
??? ?? Response.Write "확인하고자
하는
Query를
입력하세요"
??????? Response.End ' Query가
없으면
페이지
종료
??? end if
?'
연결
객체
생성
??strDsn = "driver={CUBRID Driver};server=" & strIP & ";port=" & strPort & ";uid=" & strUser & ";pwd=" & strPass & ";db_name=" & strName & ";"
' DB연결
Set DBConn = Server.CreateObject("ADODB.Connection")
?????? DBConn.Open strDsn
??? ' SQL
실행
??? Set rs = DBConn.Execute( strQuery )
??? ' SQL문에
따라
메시지
보이기
??? if InStr(Ucase(strQuery),"INSERT")>0 then
??????? Response.Write "레코드가
추가되었습니다."
??????? Response.End
??? end if
???????
??? if InStr(Ucase(strQuery),"DELETE")>0? then
??????? Response.Write "레코드가
삭제되었습니다."
??????? Response.End
??? end if
???????
??? if InStr(Ucase(strQuery),"UPDATE")>0? then
??????? Response.Write "레코드가
수정되었습니다."
??????? Response.End
??? end if???
%>
<table>
<%???
??? '
필드
이름
보여주기
??? Response.Write "<tr bgColor=#f3f3f3>"
??? For index =0 to ( rs.fields.count-1 )
??????? Response.Write "<td><b>" & rs.fields(index).name & "</b></td>"
??? Next
??? Response.Write "</tr>"
??? '
필드
값
보여주기
??? Do While Not rs.EOF
??????? Response.Write "<tr bgColor=#f3f3f3>"
??????? For index =0 to ( rs.fields.count-1 )
??????????? Response.Write "<td>" & rs(index) & "</td>"
??????? Next
??????? Response.Write "</tr>"
??????????????
??????? rs.MoveNext
??? Loop
%>
<%?
??? set? rs = nothing
%>
</table>

http://localhost/ASP수행폴더/cubrid.asp에 접속하면 수행 결과를 확인할 수 있다. 위의 ASP 예제 코드를 실행하면 다음과 같은 결과를 출력한다. 해당 항목에 알맞은 값을 넣고 Query 항목에 질의문을 입력하고 [실행하기]를 클릭하면 하단에 질의 문의 결과가 출력된다.

|image83_png|
?

**참고**
ODBC 드라이버에 대한 최신 정보를 확인하려면
`http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver>`_
에 접속한다.

**ODBC API**

ODBC API에 대한 자세한 내용은 ODBC API Reference 문서(
`http://msdn.microsoft.com/en-us/library/windows/desktop/ms714562%28v=vs.85%29.aspx <http://msdn.microsoft.com/en-us/library/windows/desktop/ms714562%28v=vs.85%29.aspx>`_
)를 참고한다. CUBRID ODBC에서 지원하는 함수 목록, ODBC Spec 버전 및 호환성은 다음과 같다.

+---------------------+------------------------+--------------------------+---------------------+
| **API**             | **Version Introduced** | **Standards Compliance** | **Support**         |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLAllocHandle      | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLBindCol          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLBindParameter    | 2.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLBrowseConnect    | 1.0                    | ODBC                     | NO                  |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLBulkOperations   | 3.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLCancel           | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLCloseCursor      | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLColAttribute     | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLColumnPrivileges | 1.0                    | ODBC                     | NO                  |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLColumns          | 1.0                    | X/Open                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLConnect          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLCopyDesc         | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLDescribeCol      | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLDescribeParam    | 1.0                    | ODBC                     | NO                  |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLDisconnect       | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLDriverConnect    | 1.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLEndTran          | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLExecDirect       | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLExecute          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLFetch            | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLFetchScroll      | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLForeignKeys      | 1.0                    | ODBC                     | YES(2008 R3.1이상 버전) |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLFreeHandle       | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLFreeStmt         | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetConnectAttr   | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetCursorName    | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetData          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetDescField     | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetDescRec       | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetDiagField     | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetDiagRec       | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetEnvAttr       | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetFunctions     | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetInfo          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetStmtAttr      | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLGetTypeInfo      | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLMoreResults      | 1.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLNativeSql        | 1.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLNumParams        | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLNumResultCols    | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLParamData        | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLPrepare          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLPrimaryKeys      | 1.0                    | ODBC                     | YES(2008 R3.1이상 버전) |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLProcedureColumns | 1.0                    | ODBC                     | YES(2008 R3.1이상 버전) |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLProcedures       | 1.0                    | ODBC                     | YES(2008 R3.1이상 버전) |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLPutData          | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLRowCount         | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetConnectAttr   | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetCursorName    | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetDescField     | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetDescRec       | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetEnvAttr       | 3.0                    | ISO 92                   | NO                  |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetPos           | 1.0                    | ODBC                     | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSetStmtAttr      | 3.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLSpecialColumns   | 1.0                    | X/Open                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLStatistics       | 1.0                    | ISO 92                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLTablePrivileges  | 1.0                    | ODBC                     | YES(2008 R3.1이상 버전) |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+
| SQLTables           | 1.0                    | X/Open                   | YES                 |
|                     |                        |                          |                     |
+---------------------+------------------------+--------------------------+---------------------+

ODBC 3.x에서 하위 호환성을 지원하지 않는 일부 함수에 대해서는 아래의 매핑 테이블을 참고하여 적합한 함수로 변환한다.

+---------------------------+-------------------+
| **ODBC 2.x 함수**           | **ODBC 3.x 함수**   |
|                           |                   |
+---------------------------+-------------------+
| SQLAllocConnect           | SQLAllocHandle    |
|                           |                   |
+---------------------------+-------------------+
| SQLAllocEnv               | SQLAllocHandle    |
|                           |                   |
+---------------------------+-------------------+
| SQLAllocStmt              | SQLAllocHandle    |
|                           |                   |
+---------------------------+-------------------+
| SQLBindParam              | SQLBindParameter  |
|                           |                   |
+---------------------------+-------------------+
| SQLColAttributes          | SQLColAttribute   |
|                           |                   |
+---------------------------+-------------------+
| SQLError                  | SQLGetDiagRec     |
|                           |                   |
+---------------------------+-------------------+
| SQLFreeConnect            | SQLFreeHandle     |
|                           |                   |
+---------------------------+-------------------+
| SQLFreeEnv                | SQLFreeHandle     |
|                           |                   |
+---------------------------+-------------------+
| SQLFreeStmt with SQL_DROP | SQLFreeHandle     |
|                           |                   |
+---------------------------+-------------------+
| SQLGetConnectOption       | SQLGetConnectAttr |
|                           |                   |
+---------------------------+-------------------+
| SQLGetStmtOption          | SQLGetStmtAttr    |
|                           |                   |
+---------------------------+-------------------+
| SQLParamOptions           | SQLSetStmtAttr    |
|                           |                   |
+---------------------------+-------------------+
| SQLSetConnectOption       | SQLSetConnectAttr |
|                           |                   |
+---------------------------+-------------------+
| SQLSetParam               | SQLBindParameter  |
|                           |                   |
+---------------------------+-------------------+
| SQLSetScrollOption        | SQLSetStmtAttr    |
|                           |                   |
+---------------------------+-------------------+
| SQLSetStmtOption          | SQLSetStmtAttr    |
|                           |                   |
+---------------------------+-------------------+
| SQLTransact               | SQLEndTran        |
|                           |                   |
+---------------------------+-------------------+

**참고**
ODBC 드라이버에 대한 최신 정보를 확인하려면
`http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver <http://www.cubrid.org/wiki_apis/entry/cubrid-odbc-driver>`_
에 접속한다.