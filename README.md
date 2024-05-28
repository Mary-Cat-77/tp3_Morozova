# Техническое задание №3 по предмету "Технологии программирования"

## Автор работы: [Морозова Мария](https://web.telegram.org/k/#@morozmarusik), 233 группа

## Описание системы:
Ниже, а также в файлах репозитория, представлены основной код программы ("Main.java") и тесты ("MainTest.java"), которые проверяют поставленное задание.

В файлах, в каталоге resource, также представлены файлы "file1.txt" и "file2.txt", на которых рекомендуется тестировать программу. Представленные текстовые файлы демонстрируют время работы программы, в зависимости от количества чисел в них. В "file1.txt" находятся 2000 чисел, в "file2.txt" - 64000 чисел.

Для проверки работоспосбости данной программы, рекомендуется скачать предложенные файлы (структура проекта Maven), для корректной работы положив все в одну папку.

Были реализованы следующие тесты: поиск максимума, поиск минимума, суммирование, умножение и определенение времени работы функции поиска максимума в зависимости от размера файла. Также, дополнительно был реализован тест, направленный на определенение времени работы функции поиска минимума в зависимости от размера файла.

## Код программы:
```java
package org.example;

import java.io.File;
import java.io.FileNotFoundException;
import java.math.BigInteger;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;


public class Main {

  public static void main(String[] args) throws Exception {
    ArrayList<BigInteger> arrayList = getDataFromFile("file1.txt");
    System.out.println("Максимум: " + _max(arrayList));
    System.out.println("Минимум: " + _min(arrayList));
    System.out.println("Сумма: " + _sum(arrayList));
    System.out.println("Произведение: " + _mult(arrayList));
  }

  public static ArrayList<BigInteger> getDataFromFile(String file_name) {
    File file = null;
    ArrayList<BigInteger> arrayList = new ArrayList<>();
    try {
      URI uri = Main.class.getResource(String.format("/%s", file_name)).toURI();
      file = new File(uri);
      Scanner scanner = new Scanner(file);
      while (scanner.hasNextInt()) {
        arrayList.add(scanner.nextBigInteger());
      }
      scanner.close();
    } catch (FileNotFoundException e) {
      System.out.println("Ошибка чтения файла " + file.getName());
    } catch (URISyntaxException e) {
      throw new RuntimeException(e);
    }
    return arrayList;
  }

  public static BigInteger _min(ArrayList<BigInteger> arrayList) {
    long timeStart = System.currentTimeMillis();
    Collections.sort(arrayList);
    BigInteger minimum = arrayList.get(0);
    long timeFinish = System.currentTimeMillis();
    System.out.println("Количество чисел в файле для функции _min = " + arrayList.size());
    System.out.println("WorkTime для функции _min = " + (timeFinish - timeStart));
    return minimum;

  }


  public static BigInteger _max(ArrayList<BigInteger> arrayList) {
    long timeStart = System.currentTimeMillis();
    Collections.sort(arrayList);
    BigInteger maximum = arrayList.get(arrayList.size() - 1);
    long timeFinish = System.currentTimeMillis();
    System.out.println("Количество чисел в файле для функции _max = " + arrayList.size());
    System.out.println("WorkTime для функции _max = " + (timeFinish - timeStart));
    return maximum;
  }


  public static BigInteger _sum(ArrayList<BigInteger> arrayList) {

    BigInteger sum = BigInteger.valueOf(0);
    for (int i = 0; i < arrayList.size(); i++) {
      sum = sum.add(arrayList.get(i));
    }

    return sum;
  }


  public static BigInteger _mult(ArrayList<BigInteger> arrayList) {
    BigInteger multi = BigInteger.valueOf(1);
    for (int i = 0; i < arrayList.size(); i++) {
      multi = multi.multiply(arrayList.get(i));
    }
    return multi;
  }

}
```

## Тесты:
```java
package org.example;

import static org.junit.Assert.*;

import org.testng.annotations.Test;

import java.math.BigInteger;
import java.util.ArrayList;

public class MainTest {

  @Test
  public void testMax() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file1.txt");
    BigInteger result = Main._max(arrayList);
    assertEquals(BigInteger.valueOf(100000), result);
  }

  @Test
  public void testMaxFile() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file2.txt");
    BigInteger result = Main._max(arrayList);
    assertEquals(BigInteger.valueOf(100000), result);
  }

  @Test
  public void testMin() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file1.txt");
    BigInteger result = Main._min(arrayList);
    assertEquals(BigInteger.valueOf(-100000), result);
  }

  @Test
  public void testMinFile() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file2.txt");
    BigInteger result = Main._min(arrayList);
    assertEquals(BigInteger.valueOf(-100000), result);
  }

  @Test
  public void testSum() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file1.txt");
    BigInteger result = Main._sum(arrayList);
    assertEquals(BigInteger.valueOf(10980), result);
  }

  @Test
  public void testMult() {
    ArrayList<BigInteger> arrayList = Main.getDataFromFile("file1.txt");
    BigInteger result = Main._mult(arrayList);
    assertEquals(new BigInteger(
            "-896616187520255193413611377023820457342240977934005775155523660768642208957712176595490131195627760438988150483221119684083863597375747085334792218922379091526814107439540147784066225406726252014890447668717953354086698948421998761815830335702993266601605891622695651009544879982081903755302821760577391552896838202530129099876928683279337394781589464077128274775660160497418434065852239766588421971233502160025151839675516874015236155784605123787318990907656411551726519542988290506479492313686389881784750193850530126790153737400451488000142593966779055988351781225705757658098985308298685270914460414341053549309979586013931970685377207624484396057143153662093985972614695599026881157738979710687508241112402735206639085735661666411945151246127724275383291748194026331699066078127713079028052333928112777828774218609396333367294022038551304001824403083214935278905195869058299869901335649646902975825891557376000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"),
        result);
  }
}
```
## График зависимости времени выполнения функции поиска максимума от количества чисел в файле:
В ходе работы над заданием был построен график зависимости функции поиска максимума от количества чисел в файле. 

В исходном файле находилось 2000 чисел. Каждый раз количество чисел в файле увеличивалось в 2 раза, за исключением последнего значения. Можем заметить, что время выполнения также увеличивается в 2 раза.
![Picture](https://github.com/Mary-Cat-77/tp2_Morozova_233/blob/b23276ce229709ae069dbdc6bcb38e238aeb34d9/Graphs.png)



