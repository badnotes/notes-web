感觉用java WebService 写 HTTP API 非常不爽

type=
@FormParam("type") @DefaultValue("1") String type,
Integer _type = Integer.parseInt(type);

java.lang.NumberFormatException: For input string: ""



type=
@FormParam("type") @DefaultValue("1") Integer type,
严重: Class java.lang.Integer can not be instantiated using a constructor with a single String argument


type=
这样传参时，@DefaultValue("1")也没用



if(null == type || "".equals(type)) type = "1";

