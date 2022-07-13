client
_______
package client2;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.Socket;

/**
 *
 * @author EASY LIFE
 */
public class TCPClient2 {
    public static void main(String argV[]) throws Exception {
        while(true){
		String CourseNum;
                String courseName;
                BufferedReader InFromUser=new BufferedReader(new InputStreamReader(System.in)); //create input stream
		Socket s=new Socket("localhost",5976);
                DataOutputStream outToServer=new DataOutputStream(s.getOutputStream());//create datatOutputStream atached to socket
                BufferedReader InFromServer=new BufferedReader(new InputStreamReader(s.getInputStream())); //create input stream
                CourseNum=InFromUser.readLine();
                outToServer.writeBytes(CourseNum+'\n');
                courseName=InFromServer.readLine();
                System.out.println(courseName);
                s.close();
        }
}}
/////////////////////////////////////////////////////////////////////

server
________
package server2;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.File;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

/**
 *
 * @author EASY LIFE
 */
public class TCPServer2 {
    public static void main(String argV[]) throws Exception{
                String err="Error 404";
                String ClientSentence;
                String Response;
                ServerSocket ss=new ServerSocket(5976);

                while(true){
		Socket s=ss.accept();
                BufferedReader inFromClient=new BufferedReader(new InputStreamReader(s.getInputStream())); //create input stream attached to socket
                DataOutputStream outToClient= new DataOutputStream(s.getOutputStream());
            
                ClientSentence=inFromClient.readLine();
                File myFile=new File("input.txt");
                Scanner myReader = new Scanner(myFile);
                while(myReader.hasNextLine()){
                    String line=myReader.nextLine();
                    String arr[]=line.split(",");
                    if (arr[0].equals(ClientSentence)){
                    Response=arr[1];
                    outToClient.writeBytes(Response+'\n');
                    break;
                    }
                    else if (myReader.hasNextLine()==false){
                        outToClient.writeBytes(err+'\n');
                    }        
                    
                }

}
    }}



