```
package boardapp.model.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import boardapp.model.dto.BoardDto;
import boardapp.util.DButil;

public class IBoardDao {

	/** 게시글 저장 */
	public void insert(BoardDto dto) throws Exception {
		String sql = "INSERT INTO board (title, writer, content, date, hitcount) VALUES (?, ?, ?, NOW(), 0)";
		try (Connection conn = DButil.getConnection(); PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setString(1, dto.getTitle());
			pstmt.setString(2, dto.getWriter());
			pstmt.setString(3, dto.getContent());
			pstmt.executeUpdate();
		}
	}

	/** 전체 게시글 조회 */
	public List<BoardDto> selectAll() throws Exception {
		List<BoardDto> list = new ArrayList<>();
		String sql = "SELECT * FROM board ORDER BY no DESC";

		try (Connection conn = DButil.getConnection();
				PreparedStatement pstmt = conn.prepareStatement(sql);
				ResultSet rs = pstmt.executeQuery()) {
			while (rs.next()) {
				BoardDto dto = new BoardDto();
				dto.setNo(rs.getInt("no"));
				dto.setTitle(rs.getString("title"));
				dto.setWriter(rs.getString("writer"));
				dto.setContent(rs.getString("content"));
//				dto.setDate(rs.getTimestamp("date").toLocalDateTime());
				dto.setHitcount(rs.getInt("hitcount"));
				list.add(dto);
			}
		}
		return list;
	}

	/** 특정 게시글 1개 조회 */
	public BoardDto selectById(int id) throws Exception {
		String sql = "SELECT * FROM board WHERE no = ?";
		try (Connection conn = DButil.getConnection(); PreparedStatement pstmt = conn.prepareStatement(sql)) {
			pstmt.setInt(1, id);
			try (ResultSet rs = pstmt.executeQuery()) {
				if (rs.next()) {
					BoardDto dto = new BoardDto();
					dto.setNo(rs.getInt("no"));
					dto.setTitle(rs.getString("title"));
					dto.setWriter(rs.getString("writer"));
					dto.setContent(rs.getString("content"));
//					dto.setDate(rs.getTimestamp("date").toLocalDateTime());
					dto.setHitcount(rs.getInt("hitcount"));
					return dto;
				}
			}
		}
		return null;
	}
}
```
