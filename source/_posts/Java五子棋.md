---
title: Java五子棋
date: 2017-10-27 18:43:50
tags: Java
category: Java学习
---

实现了准确下子、黑白交替下子、移动窗口棋子不会丢失、五连子判定、重新开始、悔棋
## Chess.java
```
package com.review;

public class Chess {
	public int x,y;
	public boolean isBlack;//表示是黑还是白
	public static int chessWidth=30;//棋子大小
	
	@Override
	public int hashCode() {
		return 1;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(this==obj){
			return true;
		}
		if(obj instanceof Chess){
			Chess c=(Chess) obj;
			if(c.x==this.x && c.y==this.y){
				return true;
			}
		}
		return false;
	}
}

```
## GameFrame.java
```
package com.hp.review;


import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JPanel;

import com.hp.test.Test;

public class GameFrame extends MainFrame {
	private GamePanel gp;
	public GameFrame(){
		//创建面板对象
		gp=new GamePanel();
		add(gp);
		//JPanel jp = (JPanel) getContentPane();
		//添加菜单栏
		JMenuBar jmb = new JMenuBar();
		setJMenuBar(jmb);
		JMenu jm = new JMenu("选项");
		jmb.add(jm);
		//为菜单添加子菜单
		JMenuItem jmi1 = new JMenuItem("重新开始");
		JMenuItem jmi2 = new JMenuItem("悔棋");
		jm.add(jmi1);
		jm.add(jmi2);
		jmi1.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				gp.restartGame();
			}
		});
		jmi2.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				gp.backPreviousStep();
			}
		});
	}
	
	public static void main(String[] args) {
		Test.setDebug(true);
		new GameFrame();
	}
}


```
## GamePanel.java
```
package com.hp.review;

import java.awt.Color;
import java.awt.Graphics;
import java.awt.Point;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import javax.swing.JOptionPane;
import javax.swing.JPanel;

import com.hp.test.Test;

public class GamePanel extends JPanel implements MouseListener {
	private int boxWidth = 48;// 每个格子的宽高
	private int startX = 20, stratY = 20;// 棋盘绘制的起点坐标
	private int hengNum = 10, shuNum = 10;// henNum表示多少行，shuNum表示多少列
	private int hang,lie;
	private boolean flag = true;// 控制绘制黑棋还是白棋的变量，为true绘制黑棋，为false绘制白棋
	private boolean isWin = false; // 如果有人胜利isWin赋值为true
	private List<Chess> chesses;
	// 指定行列值为0，则认为没有落子，如果为1咯黑子，2落了白字
	private int arr[][] = new int[hengNum + 1][shuNum + 1];// 存储棋盘上每一个交叉点信息的二维数组，hengNum=10,shuNum=10但是有11*11个点

	// 低耦合，高内聚
	public GamePanel() {
		chesses = new ArrayList<Chess>();
		setBackground(Color.YELLOW);
		addMouseListener(this);
	}

	public void paint(Graphics g) {

		super.paint(g);// 绘制面板本身
		// 绘制棋盘
		// 画10条横线
		for (int i = 0; i <= hengNum; i++) {
			g.drawLine(startX, stratY + i * boxWidth, shuNum * boxWidth
					+ startX, stratY + i * boxWidth);
		}

		// 绘制10竖线
		for (int i = 0; i <= shuNum; i++) {
			g.drawLine(startX + i * boxWidth, stratY, startX + i * boxWidth,
					hengNum * boxWidth + stratY);
		}

		// 在paint方法中绘制棋子保证最小化以后不丢失
		for (int i = 0; i < chesses.size(); i++) {
			Chess c = chesses.get(i);
			g.setColor(c.isBlack == true ? Color.black : Color.white);
			g.fillOval(c.x, c.y, c.chessWidth, c.chessWidth);
		}
	}

	// 点击
	public void mouseClicked(MouseEvent e) {
		// 如果有人获胜了不会执行此方法
		if (isWin) {
			return;
		}
		Graphics g = getGraphics();
		Point p = e.getPoint();// 鼠标点击点的坐标
		// 排除棋盘外的点击
		if (p.x < startX || p.y < stratY || p.x > (startX + boxWidth * shuNum)
				|| p.y > (stratY + boxWidth * hengNum)) {
			return;
		}
		// 确定落子的行和列
		hang = (int) Math.round((p.y - stratY) * 1.0 / boxWidth);
		lie = (int) Math.round((p.x - startX) * 1.0 / boxWidth);
		Test.print(hang + "   " + lie);

		// 确定落子交叉点的坐标
		int x = lie * boxWidth + startX - Chess.chessWidth / 2;
		int y = hang * boxWidth + stratY - Chess.chessWidth / 2;
		// 落子
		// if(flag){
		// g.setColor(new Color(0x000000));
		// }else{
		// g.setColor(new Color(0xFFFFFF));
		// }

		// 将绘制的棋子存储到集合中
		Chess c = new Chess();
		c.x = x;
		c.y = y;
		c.isBlack = flag;
		if (!chesses.contains(c)) {
			g.setColor(flag == true ? Color.BLACK : Color.WHITE);
			g.fillOval(x, y, Chess.chessWidth, Chess.chessWidth);
			arr[hang][lie] = c.isBlack == true ? 1 : 2;
			chesses.add(c);
			flag = !flag;// 必须放在循环内
		}

		// 打印输出二维数组的值
		System.out.println();
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				Test.print(arr[i][j] + "  ");
			}
			System.out.println();
		}

		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				// 横向判断
				if (j <= arr[i].length - 5) {
					if (arr[i][j] == 1 && arr[i][j + 1] == 1
							&& arr[i][j + 2] == 1 && arr[i][j + 3] == 1
							&& arr[i][j + 4] == 1) {
						System.out.println("黑棋胜利");
						this.blackWin();
						isWin = true;
						break;
					}
					if (arr[i][j] == 2 && arr[i][j + 1] == 2
							&& arr[i][j + 2] == 2 && arr[i][j + 3] == 2
							&& arr[i][j + 4] == 2) {
						System.out.println("白棋胜利");
						this.whiteWin();
						isWin = true;
						break;
					}
				}
				// 竖向判断
				if (i <= arr.length - 5) {
					if (arr[i][j] == 1 && arr[i + 1][j] == 1
							&& arr[i + 2][j] == 1 && arr[i + 3][j] == 1
							&& arr[i + 4][j] == 1) {
						System.out.println("黑棋胜利");
						this.blackWin();
						isWin = true;
						break;
					}
					if (arr[i][j] == 2 && arr[i + 1][j] == 2
							&& arr[i + 2][j] == 2 && arr[i + 3][j] == 2
							&& arr[i + 4][j] == 2) {
						System.out.println("白棋胜利");
						this.whiteWin();
						isWin = true;
						break;
					}
				}
				// /向判断,只判断右上角6*6的正方形内的所有点或者左下角6*6的正方形内的所有点
				if (j >= 4 && i <= arr.length - 5) {
					if (arr[i][j] == 1 && arr[i + 1][j - 1] == 1
							&& arr[i + 2][j - 2] == 1 && arr[i + 3][j - 3] == 1
							&& arr[i + 4][j - 4] == 1) {
						System.out.println("黑棋胜利");
						this.blackWin();
						isWin = true;
						break;
					}
					if (arr[i][j] == 2 && arr[i + 1][j - 1] == 2
							&& arr[i + 2][j - 2] == 2 && arr[i + 3][j - 3] == 2
							&& arr[i + 4][j - 4] == 2) {
						System.out.println("白棋胜利");
						this.whiteWin();
						isWin = true;
						break;
					}
				}
				// \向判断，只判断左下角6*6的正方形内的所有点
				if (i <= arr.length - 5 && j <= arr[i].length - 5) {
					if (arr[i][j] == 1 && arr[i + 1][j + 1] == 1
							&& arr[i + 2][j + 2] == 1 && arr[i + 3][j + 3] == 1
							&& arr[i + 4][j + 4] == 1) {
						System.out.println("黑棋胜利");
						this.blackWin();
						isWin = true;
						break;
					}
					if (arr[i][j] == 2 && arr[i + 1][j + 1] == 2
							&& arr[i + 2][j + 2] == 2 && arr[i + 3][j + 3] == 2
							&& arr[i + 4][j + 4] == 2) {
						System.out.println("白棋胜利");
						this.whiteWin();
						isWin = true;
						break;
					}
				}
			}
		}
	}

	public void mouseEntered(MouseEvent e) {}
	public void mouseExited(MouseEvent e) {}
	public void mousePressed(MouseEvent e) {}
	public void mouseReleased(MouseEvent e) {}

	public void blackWin() {
		Object[] options = { "确定", "再玩一把" };
		int i = JOptionPane.showOptionDialog(null, "黑棋获胜", "游戏结束",
				JOptionPane.PLAIN_MESSAGE, JOptionPane.PLAIN_MESSAGE, null,
				options, null);
		// 点确定返回0，点在玩一把返回1
		if (i == 1) {
			this.restartGame();
		}
	}

	public void whiteWin() {
		Object[] options = { "确定", "再玩一把" };
		int i = JOptionPane.showOptionDialog(null, "白棋获胜", "游戏结束",
				JOptionPane.PLAIN_MESSAGE, JOptionPane.PLAIN_MESSAGE, null,
				options, null);
		// 点确定返回0，点在玩一把返回1
		if (i == 1) {
			this.restartGame();
		}
	}

	public void restartGame() {
		// 清空数组
		chesses.clear();
		arr = null;
		arr = new int[hengNum + 1][shuNum + 1];
		flag = true;
		isWin=false;
		this.paint(getGraphics());
	}
	//悔棋
	public void backPreviousStep() {
		//移除最后下的一个棋子，更改棋盘信息
		if(!chesses.isEmpty()){
			chesses.remove(chesses.size()-1);
			arr[hang][lie]=0;
			this.paint(getGraphics());
			isWin=false; //胜利后悔棋了，还能继续下
			//悔棋后下的棋子的颜色与悔的那粒一样
			flag = flag==true?false:true;
//			if (flag) {
//				flag=false;
//			} else {
//				flag=true;
//			}
		}else{
			return;
		}
		
	}
}


```
## MainFrame.java
```
package com.review;

import java.awt.Color;
import java.awt.Container;

import javax.swing.JFrame;
import javax.swing.JPanel;

public class MainFrame extends JFrame{
	
	
	
	public MainFrame(){
		setTitle("五子棋");
		setSize(500, 500);
		setResizable(false);//设置大小不可变
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		getContentPane().setBackground(new Color(0xFF0000));
		setLocationRelativeTo(null);//设置居中
		setVisible(true);
	}

}

```






