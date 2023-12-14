---
title: Java加密
date: 2023-10-14 20:20:07
tags: Java
mathjax: true
---

# Java加密

## MD5加密

```java
import java.math.BigInteger;
import java.security.MessageDigest;
String str="..."; //明文
MessageDigest md=MessageDigest.getInstance("MD5");
md.update(str.getBytes());
System.out.println(new BigInteger(1,md.digest()).toString(16));
```

## MD5的具体实现

```java
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
public class MD5Util{
    public static String md5Encryp(String str){
        if(str==null||str.length()==0)
            throw new IllegalArgumentException("String to encript cannot be null or zero length");
        StringBuffer hexString=new StringBuffer();
        try{
            MessageDigest md=MessageDigest.getInstance("MD5");
            md.update(str.getBytes());
            byte[] hash=md.digest();
            for(int i=0;i<hash.length;i++)
                if((0xFF&hash[i])<0x10)
                    hexString.append("0"+Integer.toHexString((0xFF&hash[i])));
                else
                    hexString.append(Integer.toHexString(0xFF&hash[i]));
        }
        catch(NoSuchAlgorithmException e){
            e.printStackTrace();
        };
        return hexString.toString();
    };
};
```

## SHA加密

```java
import java.security.*;
import java.math.BigInteger;
String str="...";
MessageDigest md=MessageDigest.getInstance("SHA-1");
md.update(str.getBytes());
System.out.println(new BigInteger(1,md.digest()).toString(256));
```

## DES加密

```java
import java.security.*;
import javax.crypto.*;
import javax.crypto.spec.DESKeySpec;
import java.math.BigInteger;
public class DESEncrypt{
    public static void main(String[]args){
        String data="...";//待加密数据
        String key="12345678";//密钥，必须8的倍数长度
        //加密
        byte[] encryptData=encryptOrDecrypt(key,data.getBytes(),Cipher.ENCRYPT_MODE);
        System.out.println(new BigInteger(1,encryptData).toString(512));
        //解密
        byte[] decryptData=encryptOrDecrypt(key,encryptData,Cipher.DECRYPT_MODE);
        System.out.println(new String(decryptData));
    };
    private static byte[] encryptOrDecrypt(String key,byte[] data,int mode){
        try{
            SecureRandom secureRandom=new SecureRandom();
            DESKeySpec desKeySpec=new DESKeySpec(key.getBytes());
            SecretKeyFactory secretKeyFactory=SecretKeyFactroy.getInstance("DES");
            SecretKey secretKey=secretKeyFactory.generateSecret(desKeySpec);
            Cipher cipher=Cipher.getInstance("DES");
            cipher.init(mode,secretKey,secureRandom);
            return cipher.doFinal(data);
        }
        catch(Exception e){
            ...;
        };
    };
};
```

## AES加密

```java
import java.security.*;
import javax.crypto.*;
import javax.crypto.spec.SecretKeySpec;
import java.math.BigInteger;
public class AESEncrypt{
    public static void main(String[]args){
        String data="...";//待加密数据
        String key="12345678";//密钥，必须8的倍数长度
        //加密
        byte[] encryptData=encryptOrDecrypt(key,data.getBytes(),Cipher.ENCRYPT_MODE);
        System.out.println(new BigInteger(1,encryptData).toString(512));
        //解密
        byte[] decryptData=encryptOrDecrypt(key,encryptData,Cipher.DECRYPT_MODE);
        System.out.println(new String(decryptData));
    };
    private static byte[] encryptOrDecrypt(String key,byte[] data,int mode){
        try{
            KeyGenerator keyGenerator=KeyGenerator.getInstance("AES");
            SecureRandom random=SecureRandom.getInstance("SHAIPRNG");
            random.setSeed(key.getBytes());
            keyGenerator.init(128,random);
            SecretKey originalKey=keyGenerator.generateKey();
            byte[] rawByte=originalKey.getEncoded();
            SecretKey secretKey=new SecretKeySpec(rawByte."AES");
            Cipher cipher=Cipher.getInstance("AES");
            cipher.init(mode,secretKey);
            return cipher.doFinal(data);
        }
        catch(Exception e){
            ...;
        };
    };
};
```

## RSA加密

```java
import javax.crypto.Cipher;
import java.security.*;
import java.security.interfaces.RSAPrivateKey;
import java.security.interfaces.RSAPublicKey;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.HashMap;
import java.util.Map;
import java.math.*;
public class RSAEncrypt{
    public static final String KEY_ALGORITHM="RSA";
    private static final int KEY_SIZE=512;//密钥长度，在512~65536之间的64的倍数
    private static final String PUBLIC_KEY="RSAPublicKey";//公钥
    private static final String PRIVATE_KEY="RSAPrivateKey";//私钥
    public static Map<String,Object> initKey() throws Exception{
        //初始化密钥对
        KeyPairGenerator keyPairGenerator=KeyPairGenerator.getInstance(KEY_ALGORITHM);
        keyPairGenerator.initialize(KEY_SIZE);
        KeyPair keyPair=keyPairGenerator.generateKeyPair();
        RSAPublicKey publicKey=(RSAPublicKey)keyPair.getPublic();
        RSAPrivateKey privateKey=(RSAPrivateKey)keyPair.getPrivate();
        Map<String,Object>keyMap=new HashMap<String,Object>();
        keyMap.put(PUBLIC_KEY,publicKey);
        keyMap.put(PRIVATE_KEY,privateKey);
        return keyMap;
    };
    public static byte[] encryptByPrivateKey(byte[] data,byte[] key) throws Exception{
        //私钥加密
        PKCS8EncodedKeySpec pkcs8KeySpec=new PKCS8EncodedKeySpec(key);
        KeyFactory keyFactory=KeyFactory.getInstance(KEY_ALGORITHM);
        PrivateKey privateKey=keyFactory.generatePrivate(pkcs8KeySpec);
        Cipher cipher=Cipher.getInstance(keyFactory.getAlgorithm());
        cipher.init(Cipher.ENCRYPT_MODE,privateKey);
        return cipher.doFinal(data);
    };
    public static byte[] encryptByPublicKey(byte[] data,byte[] key) throws Exception{
        //公钥加密
        KeyFactory keyFactory=KeyFactory.getInstance(KEY_ALGORITHM);
        X509EncodedKeySpec x509KeySpec=new X509EncodedKeySpec(key);
        PublicKey pubKey=keyFactory.generatePublic(x509KeySpec);
        Cipher cipher=Cipher.getInstance(keyFactory.getAlgorithm());
        cipher.init(Cipher.ENCRYPT_MODE,pubKey);
        return cipher.doFinal(data);
    };
    public static byte[] decryptByPrivateKey(byte[] data,byte[] key) throws Exception{
        //私钥解密
        PKCS8EncodedKeySpec pkcs8KeySpec=new PKCS8EncodedKeySpec(key);
        KeyFactory keyFactory=KeyFactory.getInstance(KEY_ALGORITHM);
        PrivateKey privateKey=keyFactory.generatePrivate(pkcs8KeySpec);
        Cipher cipher=Cipher.getInstance(keyFactory.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE,privateKey);
        return cipher.doFinal(data);
    };
    public static byte[] decryptByPublicKey(byte[] data,byte[] key) throws Exception{
        //公钥解密
        KeyFactory keyFactory=KeyFactory.getInstance(KEY_ALGORITHM);
        X509EncodedKeySpec x509KeySpec=new X509EncodedKeySpec(key);
        PublicKey pubKey=keyFactory.generatePublic(x509KeySpec);
        Cipher cipher=Cipher.getInstance(keyFactory.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE,pubkey);
        return cipher.doFinal(data);
    };
    public static byte[] getPrivateKey(Map<String,Object> keyMap){
        //取得私钥
        Key key=(Key)keyMap.get(PRIVATE_KEY);
        return key.getEncoded();
    };
    public static byte[] getPublicKey(Map<String,Object> keyMap) throws Exception{
        //取得公钥
        Key key=(Key)keyMap.get(PUBLIC_KEY);
        return key.getEncoded();
    };
    public static void main(String[] args) throws Excpetion{
        Map<String,Object> keyMap=RSAEncrypt.initKey();
        byte[] publicKey=RSAEncrypt.getPublicKey(keyMap);
        byte[] privateKey=RSAEncrypt.getPrivateKey(keyMap);
        System.out.println(new BigInteger(1,publicKey).toString());//公钥
        System.out.println(new BigInteger(1,privateKey).toString());//私钥
        String str="...";//原文
        byte[] encodData1=RSAEncrypt.encryptByPrivateKey(str.getBytes(),privateKey);
        System.out.println(new BigInteger(1,encodData1).toString());//加密后数据
        byte[] decodeData1=RSAEncrypt.decryptByPublicKey(encodData1,publicKey);
        System.out.println(new String(decodeData1));//解密后数据
        //反向操作
        byte[] encodData2=RSAEncrypt.encryptByPublicKey(str.getBytes(),publicKey);
        System.out.println(new BigInteger(1,encodData2).toString());
        byte[] decodeData2=RSAEncrypt.decryptByPrivateKey(encodData2,privateKey);
        System.out.println(new String(decodeData2));
    };
};
```

## base64加密

```java
import java.io.FileInputStream;
import java.util.Base64;
//读文件
FileInputStream input=new FileInputStream("*.txt");
byte[] buffer=new byte[1024];
String str="";
while(true){
    int len=input.read(buffer);
    if(len==-1)
        break;
    str=new String(buffer,0,len);
};
//加密
final Base64.Encoder encoder=Base64.getEncoder();
final byte[] textByte=str.getBytes("UTF-8");
final String encodedText=encoder.encodeToString(textByte);
final String decodedText=new String(decoder.decode(encodedText),"UTF-8");
```

