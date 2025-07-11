#student
```
package java.jdbc.학생관리;

public class Student {
    private int stdno;
    private String stdname;
    private String phone;
    private String email;

    public Student(int stdno, String stdname, String phone, String email) {
        this.stdno = stdno;
        this.stdname = stdname;
        this.phone = phone;
        this.email = email;
    }

    // Getter methods
    public int getStdno() {
        return stdno;
    }

    public String getStdname() {
        return stdname;
    }

    public String getPhone() {
        return phone;
    }

    public String getEmail() {
        return email;
    }

    // Optional: toString for printing
    public String toString() {
        return stdno + " | " + stdname + " | " + phone + " | " + email;
    }
}
```
#studentmanager
```
package java.jdbc.학생관리;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class StudentManager {
	static final String URL = "jdbc:mysql://localhost:3306/school_db";
	static final String USER = "root"; // MySQL 사용자명
	static final String PASSWORD = "비밀번호입력"; // MySQL 비밀번호

	public static void main(String[] args) {
		try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD)) {
			// 1. 전체 목록 출력
			System.out.println("=== 전체 목록 출력 ===");
			printAll(conn);

			// 2. 오짱구 전화번호 수정
			System.out.println("\n=== 오짱구 전화번호 변경 ===");
			updatePhone(conn, "오짱구", "010-6666-7878");
			printAll(conn);

			// 3. 학생 추가
			System.out.println("\n=== 학생 추가 ===");
			insertStudent(conn, new Student(200, "유재석", "010-3626-1111", "rhu@gmail.com"));
			insertStudent(conn, new Student(300, "나검사", "010-8888-9999", "naking@naver.com"));
			printAll(conn);

			// 4. 이순신 삭제
			System.out.println("\n=== 이순신 삭제 ===");
			deleteStudent(conn, "이순신");
			printAll(conn);

			// 5. 전화번호로 학생 검색
			System.out.println("\n=== 전화번호로 검색 ===");
			findByPhone(conn, "010-3626-1111");

		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	static void printAll(Connection conn) throws SQLException {
		String sql = "SELECT * FROM student";
		try (Statement stmt = conn.createStatement(); ResultSet rs = stmt.executeQuery(sql)) {
			while (rs.next()) {
				System.out.printf("%d | %s | %s | %s\n", rs.getInt("stdno"), rs.getString("stdname"),
						rs.getString("phone"), rs.getString("email"));
			}
		}
	}

	static void updatePhone(Connection conn, String name, String newPhone) throws SQLException {
		String sql = "UPDATE student SET phone=? WHERE stdname=?";
		try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setString(1, newPhone);
			pstmt.setString(2, name);
			pstmt.executeUpdate();
		}
	}

	static void insertStudent(Connection conn, Student s) throws SQLException {
		String sql = "INSERT INTO student VALUES (?, ?, ?, ?)";
		try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setInt(1, s.getStdno());
			pstmt.setString(2, s.getStdname());
			pstmt.setString(3, s.getPhone());
			pstmt.setString(4, s.getEmail());
			pstmt.executeUpdate();
		}
	}

	static void deleteStudent(Connection conn, String name) throws SQLException {
		String sql = "DELETE FROM student WHERE stdname=?";
		try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setString(1, name);
			pstmt.executeUpdate();
		}
	}

	static void findByPhone(Connection conn, String phone) throws SQLException {
		String sql = "SELECT * FROM student WHERE phone=?";
		try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setString(1, phone);
			try (ResultSet rs = pstmt.executeQuery()) {
				if (rs.next()) {
					System.out.printf("%d | %s | %s | %s\n", rs.getInt("stdno"), rs.getString("stdname"),
							rs.getString("phone"), rs.getString("email"));
				} else {
					System.out.println("해당 전화번호를 가진 학생이 없습니다.");
				}
			}
		}
	}
}
```
