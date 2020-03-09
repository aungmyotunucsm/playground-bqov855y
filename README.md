//1.Caser cipher
#include<iostream>
#include<string.h>
using namespace std;
int main(){
 char plaintext[30],ciphertext[30],originalplaintext[30];
 
 cout<<"Enter plaintext:";
 cin>>plaintext;
 int plainlen=strlen(plaintext);
 cout<<"In encryption:\nCiphertext message=";
 for (int i=0;i<plainlen;i++)
{
 	
 	ciphertext[i]=(((plaintext[i]-'a')+3)%26)+'A';
 	cout<<ciphertext[i];
 	
 }
 cout<<"\nFor decryption:\nOriginal message=";
 for (int i=0;i<plainlen;i++)
{
 	int ans=0;
 	ans=(ciphertext[i]-'A')-3;
 	if(ans<0){
	 	
	 	originalplaintext[i]=((ans+26)%26)+'a';
	 }
	 else{
 		
 		originalplaintext[i]=(ans%26)+'a';
		}
     cout<<originalplaintext[i]; 	
 	
 }
 return 0;
 }
 
 //2.Diffie hellman
 #include<iostream>
using namespace std;
long int power(int a,int b,int mod){
long long int t;
if(b==1)
return a;
t=power(a,b/2,mod);
if(b%2==0)
return (t*t)%mod;
else
return (((t*t)%mod)*a)%mod;
}


int main(){
int x,y,p,g,a,b;
cout<<"Enter the public key p and g:"<<endl;
cin>>p;
cin>>g;
cout<<"Enter the value of first person:";
cin>>x;
cout<<"Enter the value of second person:";
cin>>y;

a=power(g,x,p);
b=power(g,y,p);
cout<<"key of first person:"<<power(b,x,p)<<endl;
cout<<"key of seconf person:"<<power(a,y,p)<<endl;

}

//3.affine cipher
#include <iostream>
#include <algorithm>
#include <numeric>
#include <cmath>
using namespace std;
int gcd(int a, int b);
int modInverse(int a, int b);

int main(){
    string choice,ans,input;
    cout << "Encrypt or Decrypt? [e/d] = ";
    cin>>choice;
    cout<<"\nInput string: ";
    cin>>input;
    int a, b;
    do{
        cout << "\na and b must be coprime\na = ";
        cin >> a;
        cout << "b = ";
        cin >> b;
    } while(cin.fail() || gcd(a,b) != 1);

    cout << '\n';

    if(choice == "e"){//Encryption
        for(int i = 0; i < input.length(); ++i){
               cout<<(char)((a * (input[i] - 'a') + b) % 26 + 'A');            
        }
        
    } else{//Decryption
        for(int i = 0; i < input.length(); ++i){
                cout << (char)(modInverse(a, 26) * (26 + input[i] - 'A' - b) % 26 + 'a');
        }
    }

    cout << '\n';
    return 0;
}

int gcd(int a, int b){
    return b == 0 ? a : gcd(b, a % b);
}

int modInverse(int a, int b){
    int b0 = b, t, q;
    int x0 = 0, x1 = 1;
    if (b == 1) return 1;
    while (a > 1) {
        q = a / b;
        t = b, b = a % b, a = t;
        t = x0, x0 = x1 - q * x0, x1 = t;
    }
    if (x1 < 0) x1 += b0;
    return x1;
}

//4.S-DES

#include<stdio.h>
int main()
{
 int i, cnt=0, p8[8]={6,3,7,4,8,5,10,9};
 int p10[10]={3,5,2,7,4,10,1,9,8,6};
 
 char input[11], k1[10], k2[10], temp[11];
 char LS1[5], LS2[5];
 //k1, k2 are for storing interim keys
 //p8 and p10 are for storing permutation key
 
 //Read 10 bits from user...
 printf("Enter 10 bits input:");
 scanf("%s",input); 
 input[10]='\0';
 
 //Applying p10...
 for(i=0; i<10; i++)
 {
  cnt = p10[i];
  temp[i] = input[cnt-1];
 }
 temp[i]='\0';
 printf("\nYour p10 key is    :");
 for(i=0; i<10; i++)
 { printf("%d,",p10[i]); }
 
 printf("\nBits after p10     :");
 puts(temp);
 //Performing LS-1 on first half of temp
 for(i=0; i<5; i++)
 {
  if(i==4)
   temp[i]=temp[0];
  else
   temp[i]=temp[i+1];   
 }
 //Performing LS-1 on second half of temp
 temp[10]=temp[5];
 for(i=5; i<10; i++)
 {
  if(i==9)
   temp[i]=temp[10];
  else
   temp[i]=temp[i+1];   
 }
 temp[i]='\0';
 printf("Output after LS-1  :");
 puts(temp);
 
 printf("\nYour p8 key is     :");
 for(i=0; i<8; i++)
 { printf("%d,",p8[i]); }

 //Applying p8...
 for(i=0; i<8; i++)
 {
  cnt = p8[i];
  k1[i] = temp[cnt-1];
 }
 k1[i]='\0';
 printf("\nYour key k1 is     :");
 puts(k1); 
//This program can be extended to generate k2 as per DES algorithm.
}

//5,6- rail-fence && transformation

#include<iostream>
using namespace std;    
#include<string.h>
int main()
{
	char strOriginalIput[100], strPass[35], strENCR[100], outpass[100];
	int istrLen=0;
	int iArray[10]={1,2,3,4,5,6,7,8,9,10};
	cout<<"\n Enter Plaintext String(without space): ";
	cin>>strOriginalIput;
	istrLen=strlen(strOriginalIput);
	strOriginalIput[istrLen+1]='\0';
	cout<<"\n Enter Key: ";
	cin>>strPass;
	int iLen=strlen(strPass);
	strPass[iLen+1]='\0';
	strcpy(outpass, strPass);
	outpass[iLen+1]='\0';
	int cnt=0;
	cout<<"Original input is: "<<strOriginalIput<<"\n";
	for(int i=0;i<iLen-1;i++)
	{
		for(int j=0;j<iLen-1-i;j++)
		{
			if(strPass[j+1]<strPass[j])
			{
				char tmp=strPass[j];
				strPass[j]=strPass[j+1];
				strPass[j+1]=tmp;
				
				int t=iArray[j];
				iArray[j]=iArray[j+1];
				iArray[j+1]=t;
					
			}
			
		}
		
	}

	
	for(int i=0;i<10;i++)
	{
		cout<<iArray[i]<<" ";
	}
	cout<<"strpass "<<strPass<<" ";
	
	cnt=0;
	for(int z=0;z<iLen;z++)
	{
		for(int x=0;x<=iLen;x++)
		{
			if((iArray[z]+iLen*x)<=istrLen)
			{
				strENCR[cnt++]=strOriginalIput[(iArray[z]+iLen*x)-1];
			}
		}
	}
	
	strENCR[istrLen]='\0';
	cout<<"\n"<<"Ciphertext string is: "<<strENCR<<"\n\n";
	int nl=1;
	for(int i=0;i<iLen;i++)
	{
		cout<<outpass[i]<<" ";
		cout<<"\n-------------------------";
		cout<<"\n";
		for(i=0;i<istrLen;i++)
		{
			if(i==iLen*nl)
			{
				cout<<"\n"<<strOriginalIput[i]<<" ";
				nl++;
			}
			else
			cout<<strOriginalIput[i]<<" ";
		}
		cout<<"\n\n"<<"\nCiphertext String: "<<strENCR;
		char strtmp[100];
		cnt=0;
		for(int z=0;z<iLen;z++)
		{
			for(int x=0;x<=iLen;x++)
			{
				if((iArray[z]+iLen*x)<=(istrLen))strtmp[iArray[z]+(iLen*x)-1]=strENCR[cnt++];
			}
		}
		strtmp[istrLen]='\0';
		cout<<"\nDecrypted String: "<<strtmp<<"\n\n";
	}
}
