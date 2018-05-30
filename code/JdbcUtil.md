```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;
import java.io.*;

public class JdbcUtil {
	private static Properties info=new Properties();
	static{	
		try {
			InputStream is=JdbcUtil.class
			   .getResourceAsStream("/com/jdbc/cfg/config.properties");
			info.load(is);
			is.close();
		} catch (Exception e) {
			throw  new ExceptionInInitializerError(e);
		}
	}
	
	private static final ThreadLocal<Connection> tl=new ThreadLocal<Connection>();
    public static Connection  getConnection() throws Exception{
    	Connection conn=tl.get();
    	if(conn==null){
    		Class.forName(info.getProperty("driver"));
    		conn=DriverManager.getConnection(info.getProperty("url"),
        			info.getProperty("username"),info.getProperty("password"));
    		tl.set(conn);
    	}
    	return conn;
    }
	public static void release(ResultSet rs,Statement stm,Connection conn){
		if(rs!=null)  try{ rs.close();} catch(Exception ee){}
		if(stm!=null)  try{ stm.close();} catch(Exception ee){}
		if(conn!=null) try{ conn.close();} catch(Exception ee){}
	}

}

```
