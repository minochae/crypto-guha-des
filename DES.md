// Arup Guha
// 11/7/06
// Solution for CIS 3362 DES Project

// There are many weaknesses in this solution due to my laziness!
// All of the constants in the algorithm should be stored in final
// static variables, but I just wanted to read in the information
// from the files instead of hard-coding them.

// Also, the key should stay the same for encrypting one file, but
// the blocks must change. This hasn't been indicated clearly.

import java.io.**;
import java.util.**;

public class DES {

> private int[.md](.md) key;
> private int[.md](.md)[.md](.md) roundkeys;
> private int[.md](.md) block;

> private static int[.md](.md)[.md](.md)[.md](.md) stables;
> private static int[.md](.md) IP;
> private static int[.md](.md) IPInv;
> private static int[.md](.md) E;
> private static int[.md](.md) PC2;
> private static int[.md](.md) P;
> private static int[.md](.md) PC1;
> private static int[.md](.md) keyshifts;

> // Reads all the information from the file I created based on the order
> // the values were stored in the file. My original posted file had some
> // errors in it, because some zeroes were stored as captial O's. I fixed
> // those issues in the file and have posted the corrected file with this
> // solution.
> public DES(int[.md](.md) thekey) throws Exception {

> key = new int[64](64.md);
> stables = new int[8](8.md)[4](4.md)[16](16.md);
> IP = new int[64](64.md);
> IPInv = new int[64](64.md);
> E = new int[48](48.md);
> PC2 = new int[48](48.md);
> P = new int[32](32.md);
> PC1 = new int[56](56.md);
> keyshifts = new int[16](16.md);
> block = new int[64](64.md);

> // Sets the key to what was passed in.
> for (int i=0; i<64; i++)
> > key[i](i.md) = thekey[i](i.md);


> Scanner fin = new Scanner(new File("destables.txt"));

> // Reads in the initial permutation matrix.
> for (int i=0; i<64; i++)
> > IP[i](i.md) = fin.nextInt();


> // Reads in the inverse of the initial permutation matrix.
> for (int i=0; i<64; i++)
> > IPInv[i](i.md) = fin.nextInt();


> // Expansion matrix used in each round.
> for (int i=0; i<48; i++)
> > E[i](i.md) = fin.nextInt();


> // The permutation matrix P used in each round.
> for (int i=0; i<32; i++)
> > P[i](i.md) = fin.nextInt();


> // Reads in the 8 S-boxes!
> for (int i=0; i<8; i++) {

> for (int j=0; j<64; j++) {
> > stables[i](i.md)[j/16][j%16] = fin.nextInt();

> }
> }

> // Reads in PC1, used for the key schedule.
> for (int i=0; i<56; i++)
> > PC1[i](i.md) = fin.nextInt();


> // Reads in PC2 used for the round keys.
> for (int i=0; i<48; i++)
> > PC2[i](i.md) = fin.nextInt();


> // Reads in the shifts used for the key between each round.
> for (int i=0; i<16; i++)
> > keyshifts[i](i.md) = fin.nextInt();


> fin.close();
> }

> // Sets a block based on the string of bits. The string is guaranteed
> // to only have characters '0' and '1'.
> public void setBlock(String bits) {
> > for (int i=0; i<64; i++)
> > > block[i](i.md) = (int)(bits.charAt(i)-'0');

> }

> // Prints out a block with spaces after every 8 characters on one line.
> public void print(FileWriter fout) throws Exception {
> > for (int i=0; i<64; i++) {
> > > if (i%8 == 0) fout.write(" ");
> > > fout.write(""+block[i](i.md));

> > }


> fout.write("\n");
> }

> // Encrypts the current block.
> public void encrypt() {

> // Permute the block with the initial permutation.
> block = Permute(block, IP);

> // Run 16 rounds.
> for (int i=0; i<16; i++) {
> > round(i);

> }

> // Supposed to switch halves at the end and invert the initial
> // permutation.
> switchHalves();
> block = Permute(block, IPInv);
> }

> // Switches the left half of the current block with the right half.
> public void switchHalves() {
> > int[.md](.md) temp = new int[32](32.md);


> // We're just doing a regular swap between 32 bits...

> for (int i=0; i<32; i++)
> > temp[i](i.md) = block[i](i.md);


> for (int i=0; i<32; i++)
> > block[i](i.md) = block[32+i];


> for (int i=32; i<64; i++)
> > block[i](i.md) = temp[i-32];

> }

> // Permutes the bits in original according to perm and
> // returns this permutation of the original bits.
> public static int[.md](.md) Permute(int[.md](.md) original, int[.md](.md) perm) {


> int[.md](.md) ans = new int[original.length];

> // Note: We subtract 1 from perm[i](i.md) because in the tables, the
> // permutations are 1-based, instead of 0-based.
> for (int i=0; i< perm.length; i++)
> > ans[i](i.md) = original[perm[i](i.md)-1];

> return ans;
> }

> // Takes the block of bits in whole from index start to index end,
> // inclusive and cyclicly left-shifts them by numbits number of bits.
> public static void leftShift(int[.md](.md) whole, int start, int end, int numbits) {
> > int size = end-start+1;
> > int[.md](.md) temp = new int[size](size.md);


> // Copy the bits into temp in their new order.
> for (int i=0; i<temp.length; i++)
> > temp[i](i.md) = whole[start+(numbits+i)%size];


> // Copy them back into the original array in the order we stored them
> // in temp, with the appropriate offset, start.
> for (int i=0; i<temp.length; i++)
> > whole[start+i] = temp[i](i.md);

> }

> public static void printArray(int[.md](.md) array) {
> > for (int i=0; i<array.length; i++) {
> > > if (i%8 == 0) System.out.print(" ");
> > > System.out.print(array[i](i.md));

> > }
> > System.out.println();

> }

> // Runs round num of DES.
> public void round(int num) {
> > int[.md](.md) left = new int[32](32.md);
> > int[.md](.md) right = new int[32](32.md);


> // Copy in the left and right blocks into temporary arrays.
> for (int i=0; i<32; i++)
> > left[i](i.md) = block[i](i.md);

> for (int i=0; i<32; i++)
> > right[i](i.md) = block[32+i];


> // Expand the right block.
> int[.md](.md) expanded = E(right);

> // This is the XOR we want.
> int[.md](.md) xorans = XOR(expanded, roundkeys[num](num.md));

> // Run the s-boxes on all the appropriate "blocks".
> int[.md](.md) sboxout = Sboxes(xorans);

> // Permute the S-box output.
> int[.md](.md) fout = Permute(sboxout, P);

> // Then do the necessary XOR.
> fout = XOR(fout, left);

> // Copy the blocks back into their proper place!
> for (int i=0; i<32; i++)
> > block[i](i.md) = right[i](i.md);

> for (int i=0; i<32; i++)
> > block[32+i] = fout[i](i.md);

> }

> // Expand the 32 bits and return the corresponding 48 bits.
> public static int[.md](.md) E(int[.md](.md) bits) {
> > int[.md](.md) ans = new int[48](48.md);


> // Our permutation function doesn't work for this, so it's coded here.
> for (int i=0; i<48; i++)
> > ans[i](i.md) = bits[E[i](i.md)-1];

> return ans;
> }

> // Returns the XOR of the bit streams a and b.
> public static int[.md](.md) XOR(int[.md](.md) a, int[.md](.md) b) {
> > int[.md](.md) ans = new int[a.length];
> > for (int i=0; i<a.length; i++)
> > > ans[i](i.md) = (a[i](i.md)+b[i](i.md))%2;

> > return ans;

> }

> // Returns the output of putting the 48 bit input through the
> // 8 S-boxes.
> public int[.md](.md) Sboxes(int[.md](.md) input) {
> > int[.md](.md) ans = new int[32](32.md);


> for (int i=0; i<8; i++) {

> // Just hard-coded this part. There doesn't seem to be a more
> // elegant way...
> int row = 2\*input[6\*i] + input[6\*i+5];
> int col = 8\*input[6\*i+1]+4\*input[6\*i+2]+2\*input[6\*i+3]+input[6\*i+4];

> int temp = stables[i](i.md)[row](row.md)[col](col.md);

> // We have to store the base-10 answer in binary, so we strip off the
> // bits one-by-one, in the usual manner from the least to most significant.
> for (int j=3; j>=0; j--) {
> > ans[4\*i+j] = temp%2;
> > temp /= 2;

> }
> }
> return ans;

> }

> // Set up the keys for each round.
> public void setKeys() {
> > roundkeys = new int[16](16.md)[48](48.md);


> // Set the original key with PC1.
> key = Permute(key, PC1);

> // Go through and set the round keys using the process by which they
> // are supposed to be computed.
> for (int i=0; i<16; i++) {

> // Supposed to left-shift both halves by the appropriate amount,
> // based on the round.
> leftShift(key, 0, 27, keyshifts[i](i.md));
> leftShift(key, 28, 55, keyshifts[i](i.md));

> // Now, just copy in the (i+1)th round key.
> for (int j=0; j<48; j++) {
> > roundkeys[i](i.md)[j](j.md) = key[PC2[j](j.md)-1];

> }
> }
> }

> // Converts the string version of the key in HEX to binary which is
> // stored in an integer array of size 64...thus, the check bits are
> // included here.
> public static int[.md](.md) getKey(String thekey) {
> > int[.md](.md) ans = new int[64](64.md);
> > thekey = thekey.toLowerCase();


> // Go through all 16 characters.
> for (int i=0; i<16; i++) {
> > int val = (int)(thekey.charAt(i));


> // We need to assign value separately if it is a digit or a letter.
> if ('0' <= val && val <= '9')
> > val = val - '0';

> else
> > val = val - 'a' + 10;


> // Peel off the binary bits as before...
> for (int j=3; j>=0; j--) {
> > ans[4\*i+j]=val%2;
> > val /= 2;

> }
> }

> return ans;
> }

> public static void main(String[.md](.md) args) throws Exception {


> Scanner stdin = new Scanner(System.in);


> // Get the necessary user information...
> System.out.println("Enter the input file.");
> String infilename = stdin.next();

> System.out.println("Enter the output file.");
> String outfilename = stdin.next();

> System.out.println("Enter the key in hex.");
> String key = stdin.next();

> Scanner fin = new Scanner(new File(infilename));
> FileWriter fout = new FileWriter(new File(outfilename));

> // Set up the keys.
> int[.md](.md) mykey = getKey(key);
> DES code = new DES(mykey);
> code.setKeys();

> // Read in each block and process...
> while (fin.hasNext()) {
> > String myblock = fin.next();


> // So we don't run into any errors!
> if (myblock == null) break;

> code.setBlock(myblock);
> code.encrypt();
> code.print(fout);
> }

> fout.close();
> fin.close();
> }

}