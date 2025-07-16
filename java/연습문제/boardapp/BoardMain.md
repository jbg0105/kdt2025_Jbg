```
package boardapp.ui;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Component;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.util.List;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableCellRenderer;

import boardapp.model.dao.IBoardDao;
import boardapp.model.dto.BoardDto;

public class BoardMain extends JFrame {
    private JTable jTable;
    private JPanel pSouth;
    private JButton btnInsert;

    public BoardMain() {
        setTitle("게시판 리스트");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        getContentPane().add(new JScrollPane(getJTable()), BorderLayout.CENTER);
        getContentPane().add(getPSouth(), BorderLayout.SOUTH);
        setSize(600, 450);
        setVisible(true);

        refreshTable(); // 생성 시 게시글 목록 불러오기
    }

    JTable getJTable() {
        if (jTable == null) {
            jTable = new JTable();

            DefaultTableModel tableModel = (DefaultTableModel) jTable.getModel();
            tableModel.addColumn("번호");
            tableModel.addColumn("제목");
            tableModel.addColumn("글쓴이");
            tableModel.addColumn("날짜");
            tableModel.addColumn("조회수");

            jTable.getColumn("번호").setPreferredWidth(20);
            jTable.getColumn("제목").setPreferredWidth(250);
            jTable.getColumn("글쓴이").setPreferredWidth(50);
            jTable.getColumn("날짜").setPreferredWidth(50);
            jTable.getColumn("조회수").setPreferredWidth(20);

            CenterTableCellRenderer ctcr = new CenterTableCellRenderer();
            jTable.getColumn("번호").setCellRenderer(ctcr);
            jTable.getColumn("제목").setCellRenderer(ctcr);
            jTable.getColumn("글쓴이").setCellRenderer(ctcr);
            jTable.getColumn("날짜").setCellRenderer(ctcr);
            jTable.getColumn("조회수").setCellRenderer(ctcr);

            jTable.addMouseListener(new MouseAdapter() {
                public void mouseClicked(MouseEvent e) {
                    // 게시글 더블 클릭 시 상세 보기 다이얼로그 열기 (추후 구현)
                }
            });
        }
        return jTable;
    }

    public class CenterTableCellRenderer extends JLabel implements TableCellRenderer {
        @Override
        public Component getTableCellRendererComponent(JTable table, Object value, boolean isSelected, boolean hasFocus,
                int row, int column) {
            setText(value == null ? "" : value.toString());
            setFont(new Font(null, Font.PLAIN, 12));
            setHorizontalAlignment(JLabel.CENTER);
            setOpaque(true);
            if (isSelected) {
                setBackground(Color.YELLOW);
            } else {
                setBackground(Color.WHITE);
            }
            return this;
        }
    }

    public JPanel getPSouth() {
        if (pSouth == null) {
            pSouth = new JPanel();
            pSouth.add(getBtnInsert());
        }
        return pSouth;

    }

    public JButton getBtnInsert() {
        if (btnInsert == null) {
            btnInsert = new JButton();
            btnInsert.setText("추가");
            btnInsert.addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    InsertDialog dialog = new InsertDialog(BoardMain.this);
                    dialog.setVisible(true);

                    if (dialog.isSaved()) {
                        System.out.println("새 글이 저장되었습니다. 테이블 갱신 필요.");
                        refreshTable(); // 저장 후 테이블 갱신
                    }
                }
            });
        }
        return btnInsert;
    }

    // 게시글 목록을 DB에서 다시 읽어와 JTable 갱신하는 메서드
    public void refreshTable() {
        DefaultTableModel model = (DefaultTableModel) jTable.getModel();
        model.setRowCount(0); // 기존 행 모두 삭제

        try {
            IBoardDao dao = new IBoardDao();
            List<BoardDto> list = dao.selectAll();
            for (BoardDto dto : list) {
                model.addRow(new Object[] { dto.getNo(), dto.getTitle(), dto.getWriter(), dto.getDate(),
                        dto.getHitcount() });
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        new BoardMain();
    }
}
```
