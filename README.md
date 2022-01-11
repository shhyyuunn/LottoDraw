# LottoDraw
로또 번호 당첨 확인 



package changnew;

import java.awt.CardLayout;
import java.awt.FlowLayout;
import java.awt.GridLayout;
import java.awt.List;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;

import javax.swing.BoxLayout;
import javax.swing.JButton;
import javax.swing.JCheckBox;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;

class LottoDrawProgram extends JFrame {
	HashMap<Integer, ArrayList<Integer>> selectMap = new HashMap<>();
	JPanel pnlMain = new JPanel(new FlowLayout());
	JPanel pnlMain2 = new JPanel();
	JPanel pnlWest = new JPanel(new GridLayout(0, 5));
	JPanel pnlEast = new JPanel();
	JPanel selectpnlmain = new JPanel();

	JPanel selectpnl1 = new JPanel();
	JPanel selectpnl2 = new JPanel();
	JPanel selectpnl3 = new JPanel();
	JPanel selectpnl4 = new JPanel();
	JPanel selectpnl5 = new JPanel();
	CardLayout card = new CardLayout();
	JPanel pnlCard = new JPanel(card);
	JLabel numLabel1, numLabel2, numLabel3, numLabel4, numLabel5;
	JPanel nlpnl1, nlpnl2, nlpnl3, nlpnl4, nlpnl5;
	JButton btnDelete1, btnDelete2, btnDelete3, btnDelete4, btnDelete5;
	JButton btnModify1 = new JButton();
	JButton btnModify2 = new JButton();
	JButton btnModify3 = new JButton();
	JButton btnModify4 = new JButton();
	JButton btnModify5 = new JButton();

	int count = 0;
	int numLabelCount;
	int winningPoint1;
	int winningPoint2;
	int winningPoint3;
	int winningPoint4;
	int winningPoint5;
	JCheckBox numberBox;

	ArrayList<Integer> numCollect = new ArrayList<>();
	ArrayList<Integer> numSelect = new ArrayList<>();
	HashMap<Integer, ArrayList<Integer>> tempMap = new HashMap<>();
	CongratulationDialog dialog = new CongratulationDialog(LottoDrawProgram.this);

	public LottoDrawProgram() {
		super("로또 번호 추첨");
		JLabel selectlbl1 = new JLabel();
		JLabel selectlbl2 = new JLabel();
		JLabel selectlbl3 = new JLabel();
		JLabel selectlbl4 = new JLabel();
		JLabel selectlbl5 = new JLabel();

		JButton auto = new JButton("자동 선택");
		JButton confirm = new JButton("번호 입력");
		JButton lottoOK = new JButton("당첨 확인");
		JButton reButton = new JButton("다시하기");
		JButton exitButton = new JButton("종료");
		btnModify1 = new JButton("수정");
		btnModify2 = new JButton("수정");
		btnModify3 = new JButton("수정");
		btnModify4 = new JButton("수정");
		btnModify5 = new JButton("수정");
		btnDelete1 = new JButton("삭제");
		btnDelete2 = new JButton("삭제");
		btnDelete3 = new JButton("삭제");
		btnDelete4 = new JButton("삭제");
		btnDelete5 = new JButton("삭제");

		ArrayList<Integer> winningNumber = new ArrayList<>();
		for (int i = 0; i < 8; i++) {
			int number = (int) Math.floor(Math.random() * 44 + 1);
			if (!winningNumber.contains(number)) {
				winningNumber.add(number);
			}
		}
		int bonusNum = winningNumber.get(6);
		winningNumber.remove(winningNumber.get(6));

		numSelect = getNumber();

		auto.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				if (auto.isEnabled()) {
					auto.setEnabled(false);
					for (; numSelect.size() < 6;) {
						Integer rand = (int) Math.floor(Math.random() * 44) + 1;
						for (int i = 0; i < numSelect.size(); i++) {
							if (rand == numSelect.get(i)) {
								rand = (int) Math.floor(Math.random() * 44) + 1;
								i = 0;
							}
						}
						numSelect.add(rand);
						count++;
					}
				} else {
					auto.setEnabled(true);
					if (count != 0) {
						for (int i = 0; i < count; i++) {
							numSelect.remove(5 - i);
						}
						count = 0;
					}
				}
			}
		});

		JLabel purchaseAmount = new JLabel();

		confirm.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				Object o = e.getSource();
				if (numSelect.size() == 6 || numCollect.size() == 6) {

					selectMap.put(numLabelCount, numSelect);
					numLabelCount++;

					Collections.sort(numSelect);
					auto.setEnabled(true);

					if (numLabel1.getText() == "A 미지정 ●  ●  ●  ●  ●  ● ") {
						if (selectMap.get(0).size() != 0) {
							numLabel1.setText("A 구매 " + selectMap.get(0));

							for (int i = 0; i < selectMap.get(0).size(); i++) {
								if (winningNumber.contains(selectMap.get(0).get(i))) {
									winningPoint1++;
								}
								if (winningPoint1 == 6) {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "1등");
								} else if (winningPoint1 == 5 && selectMap.get(0).contains(bonusNum)) {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "2등");
								} else if (winningPoint1 == 5) {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "3등");
								} else if (winningPoint1 == 4) {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "4등");
								} else if (winningPoint1 == 3) {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "5등");
								} else {
									selectlbl1.setText("A 구매 " + selectMap.get(0) + "  낙첨");
								}
							}
						} else {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 선택하세요");
						}
					} else if (numLabel2.getText() == "B 미지정 ●  ●  ●  ●  ●  ● ") {
						if (selectMap.get(1).size() != 0) {
							numLabel2.setText("B 구매 " + selectMap.get(1));
							for (int i = 0; i < selectMap.get(0).size(); i++) {
								if (winningNumber.contains(selectMap.get(0).get(i))) {
									winningPoint2++;
								}
								if (winningPoint2 == 6) {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "1등");
									dialog.setVisible(true);
								} else if (winningPoint2 == 5 && selectMap.get(0).contains(bonusNum)) {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "2등");
									dialog.setVisible(true);
								} else if (winningPoint2 == 5) {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "3등");
									dialog.setVisible(true);
								} else if (winningPoint2 == 4) {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "4등");
									dialog.setVisible(true);
								} else if (winningPoint2 == 3) {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "5등");
									dialog.setVisible(true);
								} else {
									selectlbl2.setText("B 구매 " + selectMap.get(1) + "  낙첨");
								}

							}
						} else {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 선택하세요");
						}
					} else if (numLabel3.getText() == "C 미지정 ●  ●  ●  ●  ●  ● ") {
						if (selectMap.get(2).size() != 0) {
							numLabel3.setText("C 구매 " + selectMap.get(2));
							for (int i = 0; i < selectMap.get(0).size(); i++) {
								if (winningNumber.contains(selectMap.get(0).get(i))) {
									winningPoint3++;
								}
								if (winningPoint3 == 6) {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "1등");
								} else if (winningPoint3 == 5 && selectMap.get(0).contains(bonusNum)) {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "2등");
								} else if (winningPoint3 == 5) {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "3등");
								} else if (winningPoint3 == 4) {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "4등");
								} else if (winningPoint3 == 3) {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "5등");
								} else {
									selectlbl3.setText("C 구매 " + selectMap.get(2) + "  낙첨");
								}

							}
						} else {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 선택하세요");
						}
					} else if (numLabel4.getText() == "D 미지정 ●  ●  ●  ●  ●  ● ") {
						if (selectMap.get(3).size() != 0) {
							numLabel4.setText("D 구매 " + selectMap.get(3));
							for (int i = 0; i < selectMap.get(0).size(); i++) {
								if (winningNumber.contains(selectMap.get(0).get(i))) {
									winningPoint4++;
								}
								if (winningPoint4 == 6) {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "1등");
								} else if (winningPoint4 == 5 && selectMap.get(0).contains(bonusNum)) {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "2등");
								} else if (winningPoint4 == 5) {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "3등");
								} else if (winningPoint4 == 4) {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "4등");
								} else if (winningPoint4 == 3) {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "5등");
								} else {
									selectlbl4.setText("D 구매 " + selectMap.get(3) + "  낙첨");
								}

							}
						} else {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 선택하세요");
						}
					} else if (numLabel5.getText() == "E 미지정 ●  ●  ●  ●  ●  ● ") {
						if (selectMap.get(4).size() != 0) {
							numLabel5.setText("E 구매 " + selectMap.get(4));
							for (int i = 0; i < selectMap.get(0).size(); i++) {
								if (winningNumber.contains(selectMap.get(0).get(i))) {
									winningPoint5++;

								}
								if (winningPoint5 == 6) {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "1등");
								} else if (winningPoint5 == 5 && selectMap.get(0).contains(bonusNum)) {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "2등");
								} else if (winningPoint5 == 5) {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "3등");
								} else if (winningPoint5 == 4) {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "4등");
								} else if (winningPoint5 == 3) {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "5등");
								} else {
									selectlbl5.setText("E 구매 " + selectMap.get(4) + "  낙첨");
								}

							}
						} else {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 선택하세요");
						}
					} else if (numLabelCount >= 6) {
						JOptionPane.showMessageDialog(LottoDrawProgram.this, "1회에 5개만 구매 가능");
						numLabelCount--;
					} else {
						JOptionPane.showMessageDialog(LottoDrawProgram.this, "번호를 6개까지 입력하세요");
					}
					purchaseAmount.setText("구매금액 : " + String.valueOf(numLabelCount * 1000) + "원");
					numSelect.clear();
					numSelect.removeAll(numSelect);
					pnlWest.removeAll();
					pnlWest.revalidate();
					pnlWest.repaint();
					getNumber();

				}

			}

		});

		pnlEast.setLayout(new BoxLayout(pnlEast, BoxLayout.Y_AXIS));
		JPanel pnlBottom = new JPanel();

		numLabel1 = new JLabel("A 미지정 ●  ●  ●  ●  ●  ● ");
		numLabel2 = new JLabel("B 미지정 ●  ●  ●  ●  ●  ● ");
		numLabel3 = new JLabel("C 미지정 ●  ●  ●  ●  ●  ● ");
		numLabel4 = new JLabel("D 미지정 ●  ●  ●  ●  ●  ● ");
		numLabel5 = new JLabel("E 미지정 ●  ●  ●  ●  ●  ● ");

		nlpnl1 = new JPanel();
		nlpnl2 = new JPanel();
		nlpnl3 = new JPanel();
		nlpnl4 = new JPanel();
		nlpnl5 = new JPanel();

		btnDelete1.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				if (numLabel1.getText() != "A 미지정 ●  ●  ●  ●  ●  ● ") {
					selectMap.get(0);
					numLabel1.setText("A 미지정 ●  ●  ●  ●  ●  ● ");
					nlpnl1.add(numLabel1);
					nlpnl1.add(btnModify1);
					nlpnl1.add(btnDelete1);
					selectpnlmain.remove(selectpnl1);
					purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount - 1) * 1000) + "원");
					numLabelCount--;
				} else {
					JOptionPane.showMessageDialog(LottoDrawProgram.this, "값을 입력해주세요.");
				}
			}
		});

		btnDelete2.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				if (numLabel2.getText() != "B 미지정 ●  ●  ●  ●  ●  ● ") {
					selectMap.get(1);
					numLabel2.setText("B 미지정 ●  ●  ●  ●  ●  ● ");
					nlpnl2.add(numLabel2);
					nlpnl2.add(btnModify2);
					nlpnl2.add(btnDelete2);
					selectpnlmain.remove(selectpnl2);
					purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount - 1) * 1000) + "원");
					numLabelCount--;
				} else {
					JOptionPane.showMessageDialog(LottoDrawProgram.this, "값을 입력해주세요.");
				}
			}
		});

		btnDelete3.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				if (numLabel3.getText() != "C 미지정 ●  ●  ●  ●  ●  ● ") {
					selectMap.get(2);
					numLabel3.setText("C 미지정 ●  ●  ●  ●  ●  ● ");
					nlpnl3.add(numLabel3);
					nlpnl3.add(btnModify3);
					nlpnl3.add(btnDelete3);
					selectpnlmain.remove(selectpnl3);
					purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount - 1) * 1000) + "원");
					numLabelCount--;
				} else {
					JOptionPane.showMessageDialog(LottoDrawProgram.this, "값을 입력해주세요.");
				}
			}
		});

		btnDelete4.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				if (numLabel4.getText() != "D 미지정 ●  ●  ●  ●  ●  ● ") {
					selectMap.get(3);
					numLabel4.setText("D 미지정 ●  ●  ●  ●  ●  ● ");
					nlpnl4.add(numLabel4);
					nlpnl4.add(btnModify4);
					nlpnl4.add(btnDelete4);
					selectpnlmain.remove(selectpnl4);
					purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount - 1) * 1000) + "원");
					numLabelCount--;
				} else {
					JOptionPane.showMessageDialog(LottoDrawProgram.this, "값을 입력해주세요.");
				}
			}
		});

		btnDelete5.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				if (numLabel5.getText() != "E 미지정 ●  ●  ●  ●  ●  ● ") {
					selectMap.get(4);
					numLabel5.setText("E 미지정 ●  ●  ●  ●  ●  ● ");
					nlpnl5.add(numLabel5);
					nlpnl5.add(btnModify5);
					nlpnl5.add(btnDelete5);
					selectpnlmain.remove(selectpnl5);
					purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount - 1) * 1000) + "원");
					numLabelCount--;
				} else {
					JOptionPane.showMessageDialog(LottoDrawProgram.this, "값을 입력해주세요.");
				}
			}
		});

		reButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				numLabel1.setText("A 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel2.setText("B 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel3.setText("C 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel4.setText("D 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel5.setText("E 미지정 ●  ●  ●  ●  ●  ● ");
				numLabelCount = 0;
				card.show(pnlCard, "first");

				selectlbl1.setText("");
				selectlbl2.setText("");
				selectlbl3.setText("");
				selectlbl4.setText("");
				selectlbl5.setText("");

			}
		});

		exitButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				System.exit(0);
			}
		});

		lottoOK.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseReleased(MouseEvent e) {
				card.show(pnlCard, "second");
				CongratulationDialog dialog = new CongratulationDialog(LottoDrawProgram.this);
				dialog.setVisible(true);
			}

		});

		JButton reset = new JButton("초기화");

		reset.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent arg0) {

				numLabel1.setText("A 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel2.setText("B 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel3.setText("C 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel4.setText("D 미지정 ●  ●  ●  ●  ●  ● ");
				numLabel5.setText("E 미지정 ●  ●  ●  ●  ●  ● ");
				numLabelCount = 0;
				purchaseAmount.setText("구매금액 : " + String.valueOf((numLabelCount) * 1000) + "원");

			}
		});
		nlpnl1.add(numLabel1);
		nlpnl1.add(btnModify1);
		nlpnl1.add(btnDelete1);
		nlpnl2.add(numLabel2);
		nlpnl2.add(btnModify2);
		nlpnl2.add(btnDelete2);
		nlpnl3.add(numLabel3);
		nlpnl3.add(btnModify3);
		nlpnl3.add(btnDelete3);
		nlpnl4.add(numLabel4);
		nlpnl4.add(btnModify4);
		nlpnl4.add(btnDelete4);
		nlpnl5.add(numLabel5);
		nlpnl5.add(btnModify5);
		nlpnl5.add(btnDelete5);

		pnlEast.add(nlpnl1);
		pnlEast.add(nlpnl2);
		pnlEast.add(nlpnl3);
		pnlEast.add(nlpnl4);
		pnlEast.add(nlpnl5);

		pnlBottom.add(auto);
		pnlBottom.add(confirm);
		pnlBottom.add(reset);
		pnlBottom.add(purchaseAmount);
		pnlBottom.add(lottoOK);

		pnlMain.add(pnlWest, "West");
		pnlMain.add(pnlEast, "East");
		pnlMain.add(pnlBottom);
		pnlCard.add(pnlMain, "first");
		pnlCard.add(pnlMain2, "second");

		JPanel winningpnl = new JPanel();
		JPanel main2btn = new JPanel();

		selectpnlmain.setLayout(new BoxLayout(selectpnlmain, BoxLayout.Y_AXIS));
		pnlMain2.setLayout(new BoxLayout(pnlMain2, BoxLayout.Y_AXIS));

		selectpnlmain.add(selectpnl1);
		selectpnlmain.add(selectpnl2);
		selectpnlmain.add(selectpnl3);
		selectpnlmain.add(selectpnl4);
		selectpnlmain.add(selectpnl5);
		main2btn.add(reButton);
		main2btn.add(exitButton);

		pnlMain2.add(winningpnl, "North");
		JLabel winninglbl = new JLabel();

		winningpnl.add(winninglbl);
		selectpnl1.add(selectlbl1);
		selectpnl2.add(selectlbl2);
		selectpnl3.add(selectlbl3);
		selectpnl4.add(selectlbl4);
		selectpnl5.add(selectlbl5);
		pnlMain2.add(selectpnlmain);
		pnlMain2.add(main2btn, "South");

		Collections.sort(winningNumber);
		winninglbl.setText("1등 당첨 번호 : " + winningNumber.toString() + "보너스 번호 " + bonusNum);

		add(pnlCard);

		setSize(630, 320);
		setResizable(false);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}

	public ArrayList<Integer> getNumber() {
		for (int i = 0; i < 45; i++) {
			String number = String.valueOf(i + 1);
			numberBox = new JCheckBox(number);
			pnlWest.add(numberBox);
			ItemListener item = new ItemListener() {
				@Override
				public void itemStateChanged(ItemEvent e) {
					if (e.getStateChange() == ItemEvent.SELECTED) {
						Integer numberList = Integer.valueOf(number);
						if (numCollect.size() <= 6) {
							numCollect.add(numberList);

						}
						if (numCollect.size() > 6) {
							JOptionPane.showMessageDialog(LottoDrawProgram.this, "6개까지 선택 가능");
						}
					} else if (e.getStateChange() == ItemEvent.DESELECTED) {
						Integer removNum = Integer.valueOf(((JCheckBox) e.getSource()).getText());
						numCollect.remove(removNum);
					}
				}
			};
			numberBox.addItemListener(item);
		}
		return numCollect;
	}
}

class CongratulationDialog extends JDialog {
	public CongratulationDialog(JFrame parent) {
		super(parent, "당첨을 축하드립니다!", true);
		// 당첨 등수 가져오기 및 낙첨시 미출력하게 만들기 or 낙첨시 낙첨 출력
		JPanel congratulation = new JPanel();
		JLabel congratulationlbl = new JLabel("축하");

		congratulation.add(congratulationlbl);
		add(congratulation);

		setSize(300, 300);
		setDefaultCloseOperation(DISPOSE_ON_CLOSE);
	}
}

public class Main66 {
	public static void main(String[] args) {
		LottoDrawProgram frame = new LottoDrawProgram();
		frame.setVisible(true);
	}
}
