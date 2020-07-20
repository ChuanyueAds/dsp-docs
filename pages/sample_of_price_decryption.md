# 价格解密算法示例

## 可以使用的测试数据：

    价格秘钥: tpqH5WmXWpgqNs5tcN1bnG9zscJH0p7k
    加密价格：Avl7xaaHUHoPybruXI715w
    实际价格：800

## JAVA

```java

    import com.alibaba.fastjson.JSON;
    import sun.misc.BASE64Decoder;
    import sun.misc.BASE64Encoder;
    import javax.crypto.Cipher;
    import javax.crypto.spec.SecretKeySpec;
    import java.io.IOException;
    import java.security.Security;
    import java.util.Arrays;
    import java.util.List;
    import java.util.stream.Collectors;
    
    
    public class AesUtil {
    
        /**
         * 加解密价格放在http请求中，避免出现http中关键字报错，蜻蜓方替换掉关键字
         *
         * @param data
         * @return
         */
        private static String safeUrlBase64Encode(byte[] data) {
            String encodeBase64 = new BASE64Encoder().encode(data);
            String safeBase64Str = encodeBase64.replace('+', '-');
            safeBase64Str = safeBase64Str.replace('/', '_');
            safeBase64Str = safeBase64Str.replaceAll("=", "");
            return safeBase64Str;
        }
    
        /**
         * 加解密价格放在http请求中，避免出现http中关键字报错，蜻蜓方替换掉关键字
         *
         * @param safeBase64Str
         * @return
         */
        private static byte[] safeUrlBase64Decode(final String safeBase64Str) throws IOException {
            String base64Str = safeBase64Str.replace('-', '+');
            base64Str = base64Str.replace('_', '/');
            int mod4 = base64Str.length() % 4;
            if (mod4 > 0) {
                base64Str = base64Str + "====".substring(mod4);
            }
            return new BASE64Decoder().decodeBuffer(base64Str);
        }
    
        private static String encrypt(String code, String key) throws Exception {
            byte[] enCodeFormat = key.getBytes();
            SecretKeySpec skeySpec = new SecretKeySpec(enCodeFormat, "AES");
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
            cipher.init(Cipher.ENCRYPT_MODE, skeySpec);
            byte[] encrypted = cipher.doFinal(code.getBytes("utf-8"));
            return safeUrlBase64Encode(encrypted);
        }
    
        private static String decrypt(String encode, String key) throws Exception {
            byte[] encrypted = safeUrlBase64Decode(encode);
            byte[] enCodeFormat = key.getBytes();
            SecretKeySpec skeySpec = new SecretKeySpec(enCodeFormat, "AES");
            Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
            cipher.init(Cipher.DECRYPT_MODE, skeySpec);
            byte[] original = cipher.doFinal(encrypted);
            return new String(original, "utf-8");
        }
    
        public static void main(String[] args) throws Exception {
            Security.setProperty("crypto.policy", "unlimited");
    
            String price = "800";
            String key = "tpqH5WmXWpgqNs5tcN1bnG9zscJH0p7k"; //提供给三方的密钥
    
            String enPrice = encrypt(price, key);
            System.out.println("加密结果：" + enPrice);
            System.out.println("解密结果：" + decrypt(enPrice, key));
            System.out.println("========================");
        }
    }

```

## golang

```go

package main

import (
	"bytes"
	"crypto/aes"
	"crypto/cipher"
	"crypto/rand"
	"encoding/base64"
	"encoding/binary"
	"encoding/hex"
	"errors"
	"fmt"
	"io"
	"strings"
)

func AesEncryptBase64(src, key []byte) (string, error) {
	block, err := aes.NewCipher(key) //选择加密算法
	if err != nil {
		return "", err
	}

	src = ZeroPadding(src, block.BlockSize())
	encryptText := make([]byte, aes.BlockSize+len(src))

	iv := encryptText[:aes.BlockSize]
	if _, err := io.ReadFull(rand.Reader, iv); err != nil {
		return "", err
	}

	mode := cipher.NewCBCEncrypter(block, iv)
	mode.CryptBlocks(encryptText[aes.BlockSize:], src)
	res_string := base64.URLEncoding.EncodeToString(encryptText)

	return res_string, nil
}

func AesDecryptBase64(src, key []byte) (string, error) {
	keyBytes := []byte(key)
	block, err := aes.NewCipher(keyBytes) //选择加密算法
	if err != nil {
		return "", err
	}
	tmpstr, _ := base64.URLEncoding.DecodeString(string(src))
	decrypt_Text, err := hex.DecodeString(fmt.Sprintf("%x", tmpstr))
	if err != nil {
		return "", err
	}

	// 长度不能小于aes.Blocksize
	if len(decrypt_Text) < aes.BlockSize {
		return "", errors.New("crypto/cipher: ciphertext too short")
	}

	iv := decrypt_Text[:aes.BlockSize]
	decrypt_Text = decrypt_Text[aes.BlockSize:]

	// 验证输入参数
	// 必须为aes.Blocksize的倍数
	if len(decrypt_Text)%aes.BlockSize != 0 {
		return "", errors.New("crypto/cipher: ciphertext is not a multiple of the block size")
	}
	mode := cipher.NewCBCDecrypter(block, iv)
	mode.CryptBlocks(decrypt_Text, decrypt_Text)
	ZeroUnPadding(decrypt_Text /*, block.BlockSize()*/)

	return string(decrypt_Text), nil
}

//PKCS5Padding补位
func PKCS5Padding(ciphertext []byte, blockSize int) []byte {
	padding := blockSize - len(ciphertext)%blockSize
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(ciphertext, padtext...)
}
func PKCS5UnPadding(origData []byte) []byte {
	length := len(origData)
	// 去掉最后一个字节 unpadding 次
	unpadding := int(origData[length-1])
	return origData[:(length - unpadding)]
}

//PKCS7Padding补位
func PKCS7Padding(ciphertext []byte, blockSize int) []byte {
	padding := blockSize - len(ciphertext)%blockSize
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(ciphertext, padtext...)
}
func PKCS7UnPadding(plantText []byte) []byte {
	length := len(plantText)
	unpadding := int(plantText[length-1])
	return plantText[:(length - unpadding)]
}

//Zero-Padding补位
func ZeroPadding(ciphertext []byte, blockSize int) []byte {
	padding := blockSize - len(ciphertext)%blockSize
	padtext := bytes.Repeat([]byte{0}, padding)
	return append(ciphertext, padtext...)
}
func ZeroUnPadding(origData []byte) []byte {
	length := bytes.Index(origData, []byte{0})
	//去除补位0,再返回
	return origData[:length]
}

//base64safe decode
func Base64URLDecode(data string) ([]byte, error) {
	var missing = (4 - len(data)%4) % 4
	data += strings.Repeat("=", missing)
	return base64.URLEncoding.DecodeString(data)
}

func Base64UrlSafeEncode(source []byte) string {
	// Base64 Url Safe is the same as Base64 but does not contain '/' and '+' (replaced by '_' and '-') and trailing '=' are removed.
	bytearr := base64.StdEncoding.EncodeToString(source)
	safeurl := strings.Replace(string(bytearr), "/", "_", -1)
	safeurl = strings.Replace(safeurl, "+", "-", -1)
	safeurl = strings.Replace(safeurl, "=", "", -1)
	return safeurl
}

//aes-ecb解密 (PKCS7|PKCS5|Zero)补位
func AesDecrypt(crypted, key []byte, padding string) ([]byte, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return nil, err
	}
	blockMode := NewECBDecrypter(block)
	origData := make([]byte, len(crypted))
	blockMode.CryptBlocks(origData, crypted)

	switch padding {
	case "PKCS7":
		origData = PKCS7UnPadding(origData)
	case "Zero":
		origData = ZeroUnPadding(origData)
	default:
		origData = PKCS5UnPadding(origData)
	}

	return origData, nil
}

//aes-ecb加密 (PKCS7|PKCS5|Zero)补位
func AesEncrypt(src, key, padding string) []byte {
	block, err := aes.NewCipher([]byte(key))
	if err != nil {
		fmt.Println("AesEncrypt key error1", err)
	}
	if src == "" {
		fmt.Println("AesEncrypt plain content empty")
	}
	ecb := NewECBEncrypter(block)
	content := []byte(src)
	//默认PKCS5Padding补位
	switch padding {
	case "PKCS7":
		content = PKCS7Padding(content, block.BlockSize())
	case "Zero":
		content = ZeroPadding(content, block.BlockSize())
	default:
		content = PKCS5Padding(content, block.BlockSize())
	}

	crypted := make([]byte, len(content))
	ecb.CryptBlocks(crypted, content)

	return crypted
}

type ecb struct {
	b         cipher.Block
	blockSize int
}

func newECB(b cipher.Block) *ecb {
	return &ecb{
		b:         b,
		blockSize: b.BlockSize(),
	}
}

type ecbEncrypter ecb

// NewECBEncrypter returns a BlockMode which encrypts in electronic code book
// mode, using the given Block.
func NewECBEncrypter(b cipher.Block) cipher.BlockMode {
	return (*ecbEncrypter)(newECB(b))
}

func (x *ecbEncrypter) BlockSize() int { return x.blockSize }

func (x *ecbEncrypter) CryptBlocks(dst, src []byte) {
	if len(src)%x.blockSize != 0 {
		panic("crypto/cipher: input not full blocks")
	}
	if len(dst) < len(src) {
		panic("crypto/cipher: output smaller than input")
	}
	for len(src) > 0 {
		x.b.Encrypt(dst, src[:x.blockSize])
		src = src[x.blockSize:]
		dst = dst[x.blockSize:]
	}
}

type ecbDecrypter ecb

// NewECBDecrypter returns a BlockMode which decrypts in electronic code book
// mode, using the given Block.
func NewECBDecrypter(b cipher.Block) cipher.BlockMode {
	return (*ecbDecrypter)(newECB(b))
}

func (x *ecbDecrypter) BlockSize() int { return x.blockSize }

func (x *ecbDecrypter) CryptBlocks(dst, src []byte) {
	if len(src)%x.blockSize != 0 {
		panic("crypto/cipher: input not full blocks")
	}
	if len(dst) < len(src) {
		panic("crypto/cipher: output smaller than input")
	}
	for len(src) > 0 {
		x.b.Decrypt(dst, src[:x.blockSize])
		src = src[x.blockSize:]
		dst = dst[x.blockSize:]
	}
}

func Int64ToBytes(i int64) []byte {
	var buf = make([]byte, 8)
	binary.BigEndian.PutUint64(buf, uint64(i))
	return buf
}

func TestAesDecrypt2(t *testing.T) {
	price_key := "tpqH5WmXWpgqNs5tcN1bnG9zscJH0p7k"
	encode_offer_price := AesEncrypt("800", price_key, "")
	base64_encode_offer_price := Base64UrlSafeEncode([]byte(encode_offer_price))

	println(base64_encode_offer_price) //加密后价格

	base64Decode_price, _ := Base64URLDecode(base64_encode_offer_price)
	decrypt_price, _ := AesDecrypt(base64Decode_price, []byte(price_key), "")

	println(string(decrypt_price))  //解密后的价格

}
```