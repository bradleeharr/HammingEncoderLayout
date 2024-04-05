# LEditHammingCode 
**VLSI Implementation of Hamming Encoder and Decoder Circuit using LEDIT**

# Project Description 

This project details the VLSI implementation of a Hamming (7,4), error-correcting code encoder and decoder. Within digital systems, error-correcting codes are used to reduce the probability of errors in data to create reliable transmission within noisy mediums. There are two types of error-correcting codes: Block Codes and Convolutional Codes. The Hamming (7,4) code is a simple block code that adds 3 bits of redundancy to 4-bit blocks to create 7-bit blocks.  

Error correction is essential in many digital systems to ensure data integrity. Take, for instance, the case of transmitting a character. If the character is encoded as an ASCII value, 'A' corresponds to 0x41, and in binary 01000010. If a single bit error occurs during the transmission, and the received character is now 01010010, the receiver would interpret this character now as an ‘R.’ This becomes a significant problem when applied at large scales and in noisy systems, where a valid string of characters could become entirely indecipherable. The importance of each bit is even more significant when dealing with compressed files or data, where even a single bit can be important enough to render the information invalid.

<h3> Figure 1: Transmit of Character ‘A’ and Receive of Character ‘R’ Due to Error </h3>

![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/f5dbd8e2-9df6-42ba-9c55-bb683ce6154c)


In a Hamming (7,4) code, three parity bits are mapped and added to the data block. The configuration can be seen below, where each circle has a parity bit chosen so that the number of bits in each circle is odd. 

<h3> Figure 2: Hamming (7,4) Encoding Map </h3>
<p align="center">
<img src="https://github.com/bradleeharr/LEditHammingCode/assets/56418392/3d65246c-1c81-4cf7-bb9c-55fc5970a39f">
</p>

The parity choice can be expressed in logic equations
* P5 = XOR(D1, D2, D4)
* P6 = XOR(D2, D3, D4)
* P7 = XOR(D1, D3, D4)
  
When decoding, an error can be detected if the number of bits in any of the 'circles' of the map in Figure 2 is odd. This can also be expressed as
* E1 = XOR(D1, D2, D4, P5)
* E2 = XOR(D2, D3, D4, P6)
* E3 = XOR(D1, D3, D4, P7)

After decoding, any single-bit error can be corrected based on the intersection of the erroneous 'circles', E1, E2, E3, corresponding to p5, p6, and p7 in Figure 2. This can be expressed as:
* D1 = E1 NOT(E2)E3 XOR D1
* D2 = E1E2NOT(E3) XOR D2
* D3 = NOT(E1)E2E3 XOR D3
* D4 = E1E2E3 XOR D4

# Full Logic Level Block Diagram
![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/be246517-1567-4a00-bdb2-5975e1269e1d)

# Full Logic Level Simulation 

For an example of the error-correcting properties of the encoder and decoder, the encoder is wired directly to the decoder and data bit 2 was chosen to be inverted. The output bit O2 takes some time to toggle to the correct state as the inputs change due to the delay of the input-shift register and the parity bits being recalculated, but the bit is corrected from the incorrect flip. 
<p align="center"> 
  <img src="https://github.com/bradleeharr/LEditHammingCode/assets/56418392/9f608004-42a8-4d0f-9cb6-7eb1d8f6a821">
</p>
<P align="center">
<img src="https://github.com/bradleeharr/LEditHammingCode/assets/56418392/f88052c2-cc99-4823-bc54-9a53acb0ae52">
</p>
# Cell Level Descriptions
Following are the logic cells at the tranistor level for each component used in the design. This starts with the basic Inverter up to the full circuit.
* 4.1 Inverter

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/e0753227-900f-4219-85fe-20d3631d9c51)

* 4.2 NAND2
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/c66bb4bf-6c35-481c-8858-a44887a4f45e)

* 4.3 NAND3
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/50684569-2212-495f-9ccd-077daddc417d)

* 4.4 AND3

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/f951187d-9dac-4361-b835-0a86c1f3c381)

* 4.5 XOR2
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/408ff7f3-34c0-4419-b231-2865e7237eb9)

* 4.6 XOR3

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/29f5508e-0501-4d62-b50e-7ceb2362aa0d)

* 4.7 XOR4

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/3c04883e-3f90-4e0c-8ffe-238894cfc668)

* 4.8 SR Latch
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/956109e1-ac9f-46d7-9c17-239b680c89ad)

* 4.9 Master-Slave JK Flip Flop

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/b96dab32-e1a5-4467-84b0-24a7d07d0190)

* 4.10 1x4 Serial-In-Parallel-Out Shift Register

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/4d870bc5-3a89-47c2-9923-98661774391b)

* 4.11 Parity Bit Generator
  
  This stage generates the parity bits P5, P6, P7 from data bits D1, D2, D3, D4

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/b9ede75e-44c5-41eb-8cfb-7388abf5a624)

* 4.12 Error Bit Check
  
  This stage detects errors from the 7-bit word
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/da7824fe-1c2a-4328-834c-d61f37014bf0)

* 4.13 Error Correction Stage
  
  This stage corrects errors based on the data and errors computed by the error bit check

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/f489f2c0-61c9-4195-bc2b-e2a9c13428fe)

* 4.14 Encoder
  
  The encoder performs the full encoding process
  Inputs: Data, CLK
  Outputs: D1-D4, P5-P7
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/98a8c6b2-2e58-447d-ad7e-f167586888a2)

* 4.15 Decoder
  
  The decoder performs the full decoding process
  Inputs: D1-D4, P5-P7
  Outputs: O1-O4 (The error corrected data)
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/0c5a2c09-8a7f-47dc-ac77-acc74666b3b7)

# Cell Level Layout
This consists of the cells layout in L-Edit, starting from the Inverter up to the full design.
* 6.1 Inverter
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/40c652ec-2dbe-41d2-94eb-9032848a85e9)

* 6.2 NAND2
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/5a4eba90-314f-4ccf-a6b6-09e9a3e1e245)

* 6.3 NAND3
  
   ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/cf6e2830-4757-48a6-871c-0070dd60f51f)

* 6.4 AND3

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/74b42736-640f-4bc8-b1c0-0f71d1a77a93)

* 6.5 XOR2
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/854e0bf6-47ae-439c-a37e-a281aef03b4a)

* 6.6 XOR3
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/5cde2be0-3d64-4788-a7c5-397e21f618f2)

* 6.7 XOR4

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/5b96dfbd-ecae-46b0-b4c1-503b7369a45f)

* 6.8 SR Latch
  
  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/e101b596-1a21-424e-a14d-84970ed9aa7a)

* 6.9 Master-Slave JK Flip Flop

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/be06358f-09e2-4dee-a9da-e071406033a4)

* 6.10 4x1 Serial-in-Parallel-Out Shift Register

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/3531a92b-344d-4e4b-ba7b-7f19e89b8444)

* 6.11 Parity Bit Generator

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/051088d7-8c06-4370-a372-7192294c730b)

* 6.12 Error Bit Check

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/1f669219-1512-4b48-a536-f58b4540f754)

* 6.13 Error Correction Stage

  ![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/b136f5d9-b069-4ee5-902b-e30b2ffbfe8b)

# Full Encoder Layout

![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/bd342b88-1669-4c92-a140-09ea265e3ba6)

# Full Decoder Layout
![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/b4691635-a70a-4ec5-a24c-6b09d0c59124)

# Simulation
![image](https://github.com/bradleeharr/LEditHammingCode/assets/56418392/99f3055e-02aa-4a24-b471-fe4a77a0dc9b)
