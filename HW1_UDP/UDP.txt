Server
-------
package server2;
import java.io.*;
import java.net.*;
import java.util.Scanner;
import java.util.logging.Level;
import java.util.logging.Logger;


public class UDP_Server2 {
    

   
    public static void main(String[] args)  {
        
        try {
            //String err="Error 404";
           System.out.println("wait for a connection");

            DatagramSocket serverSocket=new DatagramSocket(9436);
            byte[]recieveData=new byte[2048];
            byte[]sendData;
            while(true){
                String Response=" ";
                DatagramPacket receivePacket=new DatagramPacket(recieveData,recieveData.length);
                serverSocket.receive(receivePacket);
                String sentence =new String (receivePacket.getData());
                InetAddress IPAddress=receivePacket.getAddress();
                int port=receivePacket.getPort();
               // System.out.println("connection");
                String line=new String();
                File myFile=new File("input.txt");
                Scanner myReader = new Scanner(myFile);
                while(myReader.hasNextLine()){
                    
                  //  System.out.println("the file was opend");
                    line=myReader.nextLine();
                    String arr[]=line.split(",");
                 //    System.out.println("client said: "+sentence);
                 //    System.out.println(arr[0]+arr[1]);

                    if (arr[0].equals(sentence.trim())){
                        Response=arr[1];
                         //System.out.println("response");

                        break;
                        
                        
                    }
                    else 
                        Response="Error 404";
                   }//while

                    myReader.close();
                    sendData=Response.getBytes();
                    DatagramPacket sendPacket=new DatagramPacket(sendData, sendData.length,IPAddress,port);
                    serverSocket.send(sendPacket);
                    
                    
                
                
               // System.out.println("finish");
                
            }//while
        }//try
        catch (Exception ex) {
        }
    }//main 
    }//class
    __________________________________________________________________

client
-------
package client2;
import java.io.*;
import java.net.*;
public class UDP_Client2 {

   
    public static void main(String[] args) throws Exception {
        while(true){
        System.out.println("Enter Course Number : ");
    
        BufferedReader inFromUser= new BufferedReader(new InputStreamReader(System.in));
        DatagramSocket clientSocket = new DatagramSocket();
        InetAddress IPAddress=InetAddress.getByName("127.0.0.1");
        byte[] sendData=new byte[2048];
        byte[]recieveData=new byte[2048];
        String sentense= inFromUser.readLine();
        sendData=sentense.getBytes();
        DatagramPacket sendPacket=new DatagramPacket(sendData,sendData.length,IPAddress ,9436);
        clientSocket.send(sendPacket);
        DatagramPacket receivePacket=new DatagramPacket(recieveData,recieveData.length);
        clientSocket.receive(receivePacket);
        String OutputSentense=new String (receivePacket.getData());
        System.out.println("FromServer : "+ OutputSentense);
        clientSocket.close();
        
        
        }
        
    }
    
}
