Changes needed in code for pointing to right CAM.
Com.c we need to change in "Send_PBC_TEST_PACKET()"
In statemeachine we are sending this packet for every 1min and it can be changed.
CAM id of the METER is also set in com.c and in same above function.
It is all in HEX values. 


Steps:
1. Gateway and meter should be enabled for PBC.
2. If using local sim and local testing then we need to keep finding IP address of Meter.
3. If using US then we will have static IP sims and we can find it in Hexdump.
4. Once IP is found then login to server machine where meter is pointing and send the data through packet sender to meter IP.
5. Packet details 
00 10 C8 00 C8 00 05 00 00 00 02 D3 30 68 24 00 00 00 00 00 0A 00 00 00 00 00 00 00 00 00
Contains packet details and expiry time and amount. 
No need of CAM in this packet because we are hitting directly to meter IP.
6. **** they should enable PBC for the customer ID.

00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
00 10 C8 00 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
00 10 7D 05 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
00 10 7D 04 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00

00 10 05 7D 6A 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //US test setup 

00 10 07 7D 84 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //Ind test setup 

Direct meter pbc:
00 10 05 7D 6A 1F 0E 00 00 00 00 00 D3 30 68 40 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
00 10 C8 00 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 0A 00 00 00 00 00 00 00 00 00

Direct Meter PBC packet:
00 10 05 7D 6A 1F 0E 00 00 00 00 00 D3 30 68 40 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
									EXPIRY TIME				   AMT	

00 10 42 80 62 10 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00

SSM 
CAM --> 8069,1,32003
00 10 7D 03 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
00 10 03 7D 85 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual
      MTRID CUSID                   EXPIRY TIME                $$
 00 //actual
8068,1,32007 --1F84
00 10 07 7D 84 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
DSM 
CAM -->8069,1,35001
00 10 88 B9 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
00 10 B9 88 85 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00
DSM 
CAM -->8069,1,35002
00 10 88 BA C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
00 10 BA 88 85 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual


SSM 
CAM --> 8042,1,32005
00 10 7D 03 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
      MTRID CUSID                   EXPIRY TIME                $$
00 10 05 7D 6A 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual 
00 10 07 7D 84 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //8068-32007
00 10 08 7D 84 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //8068-32008
00 10 0B 7D 84 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //8068-32011
DSM 
CAM -->8069,1,35001
00 10 88 B9 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
00 10 B9 88 6A 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual
DSM 
CAM -->8069,1,35002
00 10 88 BA C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
00 10 BA 88 6A 1F 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual


SSM 
CAM --> 8068--1F84,1,32007 -- 7D07
00 10 7D 03 C8 00 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //refernce
CAM --> 4254 -- 109E,1,32280 -- 7E18
00 10 18 7E 9E 10 0E 00 00 00 00 00 D3 30 68 24 00 00 00 00 00 64 00 00 00 00 00 00 00 00 00 //actual
      MTRID CUSID                   EXPIRY TIME                $$




