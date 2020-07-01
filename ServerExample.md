# java
```
package server;

import java.awt.Color;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JTextField;

public class ServerExample extends JFrame{
	JPanel panel = new JPanel();
	JButton StopButton = new JButton("서버 종료 하기");
	static JTextField chat = new JTextField(30);
	ServerSocket serverSocket =null;
	Socket socket=null;
	static ArrayList<Socket> list= new ArrayList<Socket>();
	private ServerExample() {
		
		new No1().start();
		setSize(700,700);
		setTitle("Server");
		panel.setLayout(null);
		chat.setSize(700,615);
		chat.setLocation(0,0);
		chat.setEnabled(false);
		chat.setBackground(Color.black);

		StopButton.setSize(700,50);
		StopButton.setLocation(0,615);
		StopButton.addActionListener(e->{
			System.exit(0);
		});
		panel.setBackground(Color.black);
		panel.add(chat);
		panel.add(StopButton);	
		add(panel);
		setVisible(true);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}

	public static void main(String[] args) {
		ServerExample i = new ServerExample();

	}

}




```


```
package server;

import java.io.IOException;
import java.net.BindException;
import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.SocketException;

import javax.swing.JTextField;



class No1 extends Thread {
	ServerSocket serverSocket;
	Socket socket;
	String talk;
	int count=1;


	public void run()  {
			try {
				serverSocket = new ServerSocket();
				serverSocket.bind(new InetSocketAddress("localhost", 5001));
				System.out.println("서버:"+serverSocket);
				while(true) {
					socket = serverSocket.accept(); // 클라이언트의 요청을 수락
					ServerExample.list.add(socket);
					new No2(socket).start();
					count++;
				}
			}
			catch(BindException e) {
				
			}
			catch(SocketException e) {
				e.printStackTrace();
			}
			catch(IOException e) {
				e.printStackTrace(); // 안전하게 끄게 만들어줄거임
			}
	}

}

```

```
package server;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.SocketException;

public class No2 extends Thread {
	Socket socket=null;
	String talk;
	public No2(Socket socket) {
		this.socket = socket;
	}
	public void run(){

		try {
			DataInputStream din =new DataInputStream( socket.getInputStream());
			while(true){
				talk=din.readUTF();
				for(int i=0; i<ServerExample.list.size();i++) {
					DataOutputStream dos = new DataOutputStream( ServerExample.list.get(i).getOutputStream());	
					dos.writeUTF(talk);
				}
			}
		} 
		catch(SocketException e) {
			e.printStackTrace();
		}
		catch (IOException e) {
			e.printStackTrace();
		}
		

	}
}

```
