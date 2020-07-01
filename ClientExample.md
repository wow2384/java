# java
```
package client;

import java.awt.BorderLayout;
import java.awt.Color;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ConnectException;
import java.net.InetSocketAddress;
import java.net.Socket;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class ClientExample extends JFrame{
	private Socket socket=null;
	JPanel panel_DW = new JPanel();
	JPanel panel_UP = new JPanel();
	JPanel panel_CE = new JPanel();
	static JTextArea chat= new JTextArea(70,62);
	static JTextField chat1= new JTextField(30);
	JTextField Nick= new JTextField(5);
	JButton button1 = new JButton("접속 종료");
	JButton button2 = new JButton("접속하기");
	JButton button3 = new JButton("SEND");
	JLabel Nicknb= new JLabel("ID");
	JLabel Font= new JLabel("Font:");
	// JComboBox combo = new JComboBox();
	String name;
	String send;
	public static void main(String[] args) {
		ClientExample chat = new ClientExample();
	}
	private ClientExample(){
		chat.setBackground(Color.black);
		setSize(700,500);
		setTitle("chat");
		chat.setEnabled(false);
	//	combo.addItem("검정색");
	//	combo.addItem("하늘색");
	//	combo.addItem("핑크색");
	//	panel_UP.add(combo);
		panel_UP.add(Font);
		panel_UP.add(Nicknb);
		panel_UP.add(Nick);
		Nick.addActionListener(e->{
			name=Nick.getText();
		});
		button2.addActionListener(e->{
			Socket();
		});
		button3.addActionListener(e->{
			try {
				send = Nick.getText()+":"+chat1.getText()+"\n";
				chat1.setText(" ");
				DataOutputStream dos =new DataOutputStream(socket.getOutputStream());
				dos.writeUTF(send);
			}
			catch(Exception e1){
				
			}
		});
		chat.requestFocus();
		chat.setFocusable(true); 
		panel_UP.add(button2);
		button1.addActionListener(e->{
			System.exit(0);
		});
		panel_DW.add(button1);
		panel_DW.add(chat1);
		panel_CE.add(chat);
		panel_DW.add(button3);
		add(panel_UP,BorderLayout.NORTH);
		add(panel_CE,BorderLayout.CENTER);
		add(panel_DW,BorderLayout.SOUTH);
		setVisible(true);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}
	
	
	private void Socket(){
		socket =new Socket();
		try {
			socket.connect(new InetSocketAddress("localhost", 5001));
			send = Nick.getText()+"님이 입장하셨습니다. \n";
			DataOutputStream dos =new DataOutputStream(socket.getOutputStream());
			dos.writeUTF(send);
			new ClientT(socket).start();
		}
		catch(ConnectException e) {
				chat.setForeground(Color.red);
				chat.setText(chat.getText()+"\n서버가 닫혀졌습니다.");
		}
		catch(IOException e) {
			e.printStackTrace();
		}
	}
}



```

```
package client;

import java.io.DataInputStream;
import java.io.IOException;
import java.net.Socket;

public class ClientT extends Thread {
	String talk;
	Socket socket = null;
	
	public ClientT(Socket socket) {
		this.socket=socket;
	}	
	public void run(){
		try {
			DataInputStream din =new DataInputStream(socket.getInputStream());
			while(true) {
				talk = din.readUTF();
				System.out.println(talk);
				ClientExample.chat.setText(ClientExample.chat.getText()+talk);
			}
		} 
		catch (IOException e) {
			e.printStackTrace();
		}
	}
}

```


