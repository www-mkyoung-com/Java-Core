Here’s an example to show you how to select the entire records from table via JDBC statement, and display all the records via a ResultSet object. To issue a select query, calls the `Statement.executeQuery` method like this :

    String selectTableSQL = "SELECT USER_ID, USERNAME from DBUSER";
    Statement statement = dbConnection.createStatement();
    ResultSet rs = statement.executeQuery(selectTableSQL);
    while (rs.next()) {
    	String userid = rs.getString("USER_ID");
    	String username = rs.getString("USERNAME");
    }

_Full example…_

    package com.mkyong.jdbc;

    import java.sql.DriverManager;
    import java.sql.Connection;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import java.sql.Statement;

    public class JDBCStatementSelectExample {

    	private static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";
    	private static final String DB_CONNECTION = "jdbc:oracle:thin:@localhost:1521:MKYONG";
    	private static final String DB_USER = "user";
    	private static final String DB_PASSWORD = "password";

    	public static void main(String[] argv) {

    		try {

    			selectRecordsFromDbUserTable();

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		}

    	}

    	private static void selectRecordsFromDbUserTable() throws SQLException {

    		Connection dbConnection = null;
    		Statement statement = null;

    		String selectTableSQL = "SELECT USER_ID, USERNAME from DBUSER";

    		try {
    			dbConnection = getDBConnection();
    			statement = dbConnection.createStatement();

    			System.out.println(selectTableSQL);

    			// execute select SQL stetement
    			ResultSet rs = statement.executeQuery(selectTableSQL);

    			while (rs.next()) {

    				String userid = rs.getString("USER_ID");
    				String username = rs.getString("USERNAME");

    				System.out.println("userid : " + userid);
    				System.out.println("username : " + username);

    			}

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		} finally {

    			if (statement != null) {
    				statement.close();
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

    			dbConnection = DriverManager.getConnection(DB_CONNECTION, DB_USER,
    					DB_PASSWORD);
    			return dbConnection;

    		} catch (SQLException e) {

    			System.out.println(e.getMessage());

    		}

    		return dbConnection;

    	}

    }

## Result

A list of the records are retrieved from table “DBUSER” and displayed.

[http://www.mkyong.com/jdbc/jdbc-statement-example-select-list-of-the-records/](http://www.mkyong.com/jdbc/jdbc-statement-example-select-list-of-the-records/)
