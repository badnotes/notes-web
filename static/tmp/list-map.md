list-map
ArrayList HashMap
java程序中常常会用ArrayList，有时想用一行完成ArrayList的初始化赋值。主要有2种方法。

法一：
利用Array与ArrayList的相互转换方法，代码如下：

ArrayList<String> list = new ArrayList(Arrays.asList("Ryan", "Julie", "Bob")); 
法二:
利用ArrayList的add方法完成初始化赋值，代码如下：

List list = new ArrayList<String>(){{
add("A");
add("B");
}}

//初始化List
List<string> list   = new ArrayList</string><string>();
list.add("string1");
list.add("string2");
//some other list.add() code......
list.add("stringN");
 
//初始化Map
Map</string><string , String> map = new HashMap</string><string , String>();
map.put("key1", "value1");
map.put("key2", "value2");
//.... some other map.put() code
map.put("keyN", "valueN");
</string>


//初始化List
List<string> list   = new ArrayList</string><string>(){{
    add("string1");
    add("string2");
    //some other add() code......
    add("stringN");
}};
 
//初始化Map
Map</string><string , String> map = new HashMap</string><string , String>(){{
    put("key1", "value1");
    put("key2", "value2");
    //.... some other put() code
    put("keyN", "valueN");
}};
</string>

String[] array = (String[])list.toArray(new String[size]); 


Array to Map

String[][] countries = { { "United States", "New York" }, { "United Kingdom", "London" }, 
        { "Netherland", "Amsterdam" }, { "Japan", "Tokyo" }, { "France", "Paris" } }; 
  
    Map countryCapitals = ArrayUtils.toMap(countries);  


List<String> list = Arrays.asList("王利虎","张三","李四"); 

relist = Lists.reverse(list); // 返回新列表

ArrayUtils.reverse(value); // 反排数组