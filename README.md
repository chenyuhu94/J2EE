#帮助类  
##1.MD5Utils.java


```
import java.security.MessageDigest;
public class MD5Utils {
   public final static String MD5(String s) {
      char hexDigits[] = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
            'a', 'b', 'c', 'd', 'e', 'f' };
      try {
         byte[] strTemp = s.getBytes();
         MessageDigest mdTemp = MessageDigest.getInstance("MD5");
         mdTemp.update(strTemp);
         byte[] md = mdTemp.digest();
         int j = md.length;
         char str[] = new char[j * 2];
         int k = 0;
         for (int i = 0; i < j; i++) {
            byte byte0 = md[i];
            str[k++] = hexDigits[byte0 >>> 4 & 0xf];
            str[k++] = hexDigits[byte0 & 0xf];
         }
         return new String(str);
      } catch (Exception e) {
         return null;
      }
   }
}
```
##2.SendMail.java  
```
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;
import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CheckCodeServlet extends HttpServlet {
   private String letters = "0123456789ABCDEFGHIGKLMNOPQRSTUVWXYZ";
   public void doGet(HttpServletRequest request, HttpServletResponse response)
         throws ServletException, IOException {
      this.doPost(request, response);
   }
   public void doPost(HttpServletRequest request, HttpServletResponse response)
         throws ServletException, IOException {
      // 禁止浏览器缓存随机图片
      response.setDateHeader("Expires", -1);
      response.setHeader("Cache-Control", "no-cache");
      response.setHeader("Pragma", "no-cache");
      // 准备一个画板(验证码图片的大小)
      int width = 240;
      int height = 60;
      // 验证码的个数
      int n = 6;
      // 验证码的间隙
      int space = 5;
      // 验证码据两端的位置
      int boder = 10;
      BufferedImage image = new BufferedImage(width, height,
            BufferedImage.TYPE_INT_RGB);
      // 获取一个画笔
      Graphics g = image.getGraphics();
      // 设置画笔颜色
      g.setColor(this.getColor());
      // 绘制滑板的背景颜色
      g.fillRect(0, 0, width, height);
      // 防止验证码被解析
      Random random = new Random();
      for (int i = 0; i < 100; i++) {
         g.setColor(getColor());
         int x = random.nextInt(width);
         int y = random.nextInt(height);
         g.drawOval(x, y, 2, 2);
      }
      String code = "";
      // 生成字母
      // 每个字符的宽度
      int lw = (width - boder * 2 - space * (n - 1)) / n;
      for (int i = 0; i < n; i++) {
         String letter = this.getLetter();
         code += letter;
         g.setColor(this.getColor());
         g.setFont(new Font("华文行楷", Font.BOLD, 40));
         g.drawString(letter, boder + i * (lw + space), height - boder);
      }
      request.getSession().setAttribute("code", code);
      // 5.把写好数据的图片输出给浏览器
      ImageIO.write(image, "jpg", response.getOutputStream());
   }
   /**
    * 产生一个随机数
    */

   private Color getColor() {
      Random random = new Random();
      int r = random.nextInt(256);
      int g = random.nextInt(256);
      int b = random.nextInt(256);
      return new Color(r, g, b);
   }
   
   public String getLetter() {
      Random random = new Random();
      int index = random.nextInt(letters.length());
      return letters.charAt(index) + "";
   }
}
```
 

