```
package boardapp.ui;

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.FlowLayout;

import javax.swing.Box;
import javax.swing.BoxLayout;
import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.border.EmptyBorder;
import javax.swing.border.EtchedBorder;

import boardapp.model.dao.IBoardDao;
import boardapp.model.dto.BoardDto;

public class InsertDialog extends JDialog {

    private JTextField txtTitle, txtWriter;
    private JTextArea txtContent;
    private boolean saved = false;

    public InsertDialog(JFrame parent) {
        super(parent, "게시물 작성", true);
        setSize(400, 300);
        setLocationRelativeTo(parent);
        setLayout(new BorderLayout());
        setDefaultCloseOperation(DISPOSE_ON_CLOSE);

        add(getPCenter(), BorderLayout.CENTER);
        add(getPSouth(), BorderLayout.SOUTH);
    }

    private JPanel getPCenter() {
        JPanel p = new JPanel();
        p.setLayout(new BoxLayout(p, BoxLayout.Y_AXIS));
        p.setBorder(new EmptyBorder(10, 10, 10, 10));

        // 제목
        JPanel row1 = new JPanel(new BorderLayout());
        row1.add(new JLabel("제목", JLabel.CENTER), BorderLayout.WEST);
        txtTitle = new JTextField();
        row1.add(txtTitle, BorderLayout.CENTER);
        row1.setMaximumSize(new Dimension(380, 30));
        p.add(row1);
        p.add(Box.createVerticalStrut(10));

        // 글쓴이
        JPanel row2 = new JPanel(new BorderLayout());
        row2.add(new JLabel("글쓴이", JLabel.CENTER), BorderLayout.WEST);
        txtWriter = new JTextField();
        row2.add(txtWriter, BorderLayout.CENTER);
        row2.setMaximumSize(new Dimension(380, 30));
        p.add(row2);
        p.add(Box.createVerticalStrut(10));

        // 내용
        JPanel row3 = new JPanel(new BorderLayout());
        row3.add(new JLabel("내용", JLabel.CENTER), BorderLayout.WEST);
        txtContent = new JTextArea(5, 20);
        txtContent.setBorder(new EtchedBorder());
        txtContent.setLineWrap(true);
        txtContent.setWrapStyleWord(true);
        JScrollPane scroll = new JScrollPane(txtContent);
        row3.add(scroll, BorderLayout.CENTER);
        row3.setMaximumSize(new Dimension(380, 120));
        p.add(row3);

        return p;
    }

    private JPanel getPSouth() {
        JPanel south = new JPanel(new FlowLayout(FlowLayout.RIGHT));
        JButton btnOk = new JButton("저장");
        JButton btnCancel = new JButton("취소");

        btnOk.addActionListener(e -> {
            if (txtTitle.getText().trim().isEmpty() ||
                txtWriter.getText().trim().isEmpty() ||
                txtContent.getText().trim().isEmpty()) {
                JOptionPane.showMessageDialog(this, "모든 항목을 입력하세요.");
                return;
            }

            try {
                BoardDto dto = new BoardDto();
                dto.setTitle(txtTitle.getText().trim());
                dto.setWriter(txtWriter.getText().trim());
                dto.setContent(txtContent.getText().trim());

                IBoardDao dao = new IBoardDao();
                dao.insert(dto);

                saved = true;

                // 저장 성공 후 부모 창(BoardMain)의 refreshTable() 호출
                if (saved) {
                    ((BoardMain) getParent()).refreshTable();
                }

                dispose();

            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "저장 중 오류: " + ex.getMessage());
                ex.printStackTrace();
            }
        });

        btnCancel.addActionListener(e -> dispose());

        south.add(btnOk);
        south.add(btnCancel);
        return south;
    }

    public boolean isSaved() {
        return saved;
    }
}
```
