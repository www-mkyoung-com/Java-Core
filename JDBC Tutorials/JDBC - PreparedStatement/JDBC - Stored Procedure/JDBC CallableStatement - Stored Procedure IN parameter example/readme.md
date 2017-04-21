Code snippets to show you how to call a Oracle stored procedure via JDBC `CallableStatement`, and how to pass IN parameters from Java to stored procedure.

    //insertDBUSER is stored procedure
    String insertStoreProc = "{call insertDBUSER(?,?,?,?)}";
    callableStatement = dbConnection.prepareCall(insertStoreProc);
    callableStatement.setInt(1, 1000);
    callableStatement.setString(2, "mkyong");
    callableStatement.setString(3, "system");
    callableStatement.setDate(4, getCurrentDate());
    callableStatement.executeUpdate();

## JDBC CallableStatement Example

See a full JDBC `CallableStatement` example.

## 1\. Stored Procedure

A stored procedure in Oracle database. Later, calls it via JDBC.

    CREATE OR REPLACE PROCEDURE insertDBUSER(
    	   p_userid IN DBUSER.USER_ID%TYPE,
    	   p_username IN DBUSER.USERNAME%TYPE,
    	   p_createdby IN DBUSER.CREATED_BY%TYPE,
    	   p_date IN DBUSER.CREATED_DATE%TYPE)
    IS
    BEGIN

      INSERT INTO DBUSER ("USER_ID", "USERNAME", "CREATED_BY", "CREATED_DATE")
      VALUES (p_userid, p_username,p_createdby, p_date);

      COMMIT;

    END;
    /

## 2\. Calls Stored Procedure via CallableStatement

JDBC example to call stored procedure via `CallableStatement`.

_File : JDBCCallableStatementINParameterExample.java_

    package com.mkyong.jdbc;

    import java.sql.CallableStatement;
    import java.sql.DriverManager;
    import java.sql.Connection;
    import java.sql.SQLException;

    public class JDBCCallableStatementINParameterExample {

    	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
    	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
    	private static final String DB_USER = "user";
    	private static final String DB_PASSWORD = "password";

    	public static void main(String[] argv) {

    		try {

    			callOracleStoredProcINParameter();

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		}

    	}

    	private static void callOracleStoredProcINParameter() throws SQLException {

    		Connection dbConnection = null;
    		CallableStatement callableStatement = null;

    		String insertStoreProc = "{call insertDBUSER(?,?,?,?)}";

    		try {
    			dbConnection = getDBConnection();
    			callableStatement = dbConnection.prepareCall(insertStoreProc);

    			callableStatement.setInt(1, 1000);
    			callableStatement.setString(2, "mkyong");
    			callableStatement.setString(3, "system");
    			callableStatement.setDate(4, getCurrentDate());

    			// execute insertDBUSER store procedure
    			callableStatement.executeUpdate();

    			System.out.println("Record is inserted into DBUSER table!");

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		} finally {

    			if (callableStatement != null) {
    				callableStatement.close();
    			}

    			if (dbConnection != null) {
    				dbConnection.close();
    			}

    		}

    	}

    	private static Connection getDBConnection() {

    		Connection dbConnection = null;

    		try {

    			Class.forName(DB_DRIVER);

    		} catch (ClassNotFoundException e) {

    			System.out.println(e.getMessage());

    		}

    		try {

    			dbConnection = DriverManager.getConnection(
    				DB_CONNECTION, DB_USER,DB_PASSWORD);
    			return dbConnection;

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		}

    		return dbConnection;

    	}

    	private static java.sql.Date getCurrentDate() {
    		java.util.Date today = new java.util.Date();
    		return new java.sql.Date(today.getTime());
    	}

    }

Done. When above JDBC example is executed, a new record will be inserted into database via stored procedure “`insertDBUSER`“.

## Reference

1.  [http://download.oracle.com/javase/6/docs/api/java/sql/CallableStatement.html](http://download.oracle.com/javase/6/docs/api/java/sql/CallableStatement.html)
2.  [http://docsrv.sco.com/JDK_guide/jdbc/getstart/callablestatement.doc.html](http://docsrv.sco.com/JDK_guide/jdbc/getstart/callablestatement.doc.html)
3.  [http://onjava.com/pub/a/onjava/2003/08/13/stored_procedures.html](http://onjava.com/pub/a/onjava/2003/08/13/stored_procedures.html)

[http://www.mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-in-parameter-example/](http://www.mkyong.com/jdbc/jdbc-callablestatement-stored-procedure-in-parameter-example/)
