```
package streamproject.sec04;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Component;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableCellRenderer;

import streamproject.sec03.Nation;

public class NationApp extends JFrame {

	private final JButton btnSort;
	private JTable jTable;
	JComboBox<String> cmbOrder;
	DefaultTableModel tableModel;

	NationApp() {
		setTitle("국가");

		add(new JScrollPane(makeTable()), BorderLayout.CENTER);

		JPanel panel = new JPanel();
		add(panel, BorderLayout.SOUTH);

		String[] cmbString = { "국가별", "인구수", "GDP" };
		cmbOrder = new JComboBox<String>(cmbString);

		panel.add(cmbOrder);

		btnSort = new JButton("정렬");
		panel.add(btnSort);

		btnSort.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				sortTable();
			}
		});

		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setSize(600, 250);
		setVisible(true);
	}

	JTable makeTable() {
		if (jTable == null) {
			jTable = new JTable();

			tableModel = (DefaultTableModel) jTable.getModel();
			tableModel.addColumn("번호");
			tableModel.addColumn("국가명");
			tableModel.addColumn("유형");
			tableModel.addColumn("인구수");
			tableModel.addColumn("GDP순위");

			jTable.getColumn("번호").setPreferredWidth(30);
			jTable.getColumn("국가명").setPreferredWidth(100);
			jTable.getColumn("유형").setPreferredWidth(30);
			jTable.getColumn("인구수").setPreferredWidth(30);
			jTable.getColumn("GDP순위").setPreferredWidth(30);

			CenterTableCellRenderer ctcr = new CenterTableCellRenderer();
			for (int i = 0; i < jTable.getColumnCount(); i++) {
				jTable.getColumnModel().getColumn(i).setCellRenderer(ctcr);
			}

			// 초기 데이터 표시
			fillTable(Nation.nations);
		}
		return jTable;
	}

	void fillTable(List<Nation> list) {
		tableModel.setRowCount(0); // 기존 행 삭제
		int idx = 1;
		for (Nation n : list) {
			tableModel.addRow(new Object[] {
					idx++,
					n.getName(),
					n.getType().toString(),
					n.getPopulation(),
					n.getGdpRank()
			});
		}
	}

	void sortTable() {
		String selected = (String) cmbOrder.getSelectedItem();
		List<Nation> sortedList = null;

		switch (selected) {
			case "국가별":
				sortedList = Nation.nations.stream()
						.sorted(Comparator.comparing(Nation::getName))
						.collect(Collectors.toList());
				break;
			case "인구수":
				sortedList = Nation.nations.stream()
						.sorted(Comparator.comparing(Nation::getPopulation).reversed())
						.collect(Collectors.toList());
				break;
			case "GDP":
				sortedList = Nation.nations.stream()
						.sorted(Comparator.comparing(Nation::getGdpRank))
						.collect(Collectors.toList());
				break;
		}

		if (sortedList != null) {
			fillTable(sortedList);
		}
	}

	public class CenterTableCellRenderer extends JLabel implements TableCellRenderer {
		public Component getTableCellRendererComponent(JTable table, Object value, boolean isSelected,
				boolean hasFocus, int row, int column) {
			setText(value.toString());
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

	public static void main(String[] args) {
		new NationApp();
	}
}
```
