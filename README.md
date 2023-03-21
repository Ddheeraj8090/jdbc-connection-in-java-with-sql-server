
import java.util.Scanner;
import java.sql.*;
import java.io.*;
import product.*;

public class Main {
    static void login() {

        try {
            System.out.println("Enter Your Email");
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            String email=br.readLine();
            System.out.println("Enter Your Password");
            String pass = br.readLine();
            String url = "jdbc:mysql://localhost:3306/project";
            String username = "root";
            String password = "";

            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection(url, username, password);

            Statement statement = connection.createStatement();
            /*PreparedStatement st = (PreparedStatement) connection
                    .prepareStatement("Select EMAIL, password from singup where EMAIL=? and password=?");
            st.setString(1, username);
            st.setString(2, password);*/
            String q ="Select email,password from singup where email='"+email+"' and password='"+pass+"'";
            ResultSet rs= statement.executeQuery(q);
            if (rs.next()) {
                System.out.println("login Successfully");
                product l = new product();
            } else {
                System.out.println("not login");
            }

        } catch (Exception e) {
            System.out.println(e);
        }


    }

    public static void main(String[] args) {
        System.out.println("your are already singUp Y or N Enter : ");
        String url = "jdbc:mysql://localhost:3306/project";
        String username = "root";
        String password = "";
        Scanner sc = new Scanner(System.in);
        char losin = sc.next().charAt(0);

        if (losin == 'y') {
            login();
        } else {
            try {


                Class.forName("com.mysql.cj.jdbc.Driver");
                Connection connection = DriverManager.getConnection(url, username, password);

                Statement statement = connection.createStatement();

                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

                System.out.println("Enter Your Name:");
                String name = br.readLine();

                System.out.println("Enter Your Email");
                String email = br.readLine();

                System.out.println("Enter Your Mobile No.");
                String mobile = br.readLine();

                System.out.println("enter Your address ");
                String address = br.readLine();
                System.out.println("Enter Your Password");
                String pass=br.readLine();


                String qqq = "insert into singup(name,email,mobileno,address,password) values(?,?,?,?,?)";

                PreparedStatement pst = connection.prepareStatement(qqq);

                pst.setString(1, name);
                pst.setString(2, email);
                pst.setString(3, mobile);
                pst.setString(4, address);
                pst.setString(5,pass);

                String co = "SELECT COUNT(*) FROM singup where email='" + email + "'";
                ResultSet rs = statement.executeQuery(co);
                rs.next();
                String Countrow = rs.getString(1);
                if (Countrow.equals("0")) {
                    pst.executeUpdate();
                    System.out.println("SignUp Successfully");
                    System.out.println("login");
                    login();
                } else {
                    System.out.println("User Email already exists !");

                }


            } catch (Exception e) {
                System.out.println(e);
            }
        }
    }
}
