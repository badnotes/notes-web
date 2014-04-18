---
layout: post
title: ""
tagline: "New Post"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。"
category: 
tags: []
---
{% include JB/setup %}


 @Autowired
    AnnouncementTransportDao announcementDao;
    String sql = "select ob_textid_0042, f001v_0042,f002d_0042, f004v_0042, f006b_0042, f008v_0042, ob_isvalid_0042 from juchao.tb_text_0042 where ob_textid_0042 = ?";

    /**
     * 出现乱码的原因是编码判断错误
     */
    @Test
    public void oracleTest(){
        //  sid: 63853774  _id: 534f3a5545ce077da23ee404  正常
        //  sid: 63848774  _id: 534de90b45ced9b63ae7d479  乱码
        //  sid: 63848773  乱
        int id = 63848773;
        announcementDao.getJdbcTemplate().query(sql,new RowCallbackHandler() {
            @Override
            public void processRow(ResultSet rs) throws SQLException {
                Blob content = rs.getBlob("f006b_0042");
                if(content != null){

                    write(content);

                    //writeFile(content);

                    //writeToFile(content);
                }
            }

            // 输出到文件 不会乱码
            void writeFile(Blob content) throws SQLException {
                BufferedInputStream ins = new BufferedInputStream(content.getBinaryStream());
                BufferedOutputStream out;
                try {
                    out = new BufferedOutputStream(new FileOutputStream("D:/wanjun/test_blob.txt"));
                    int c;
                    while ((c=ins.read()) != -1) {
                        out.write(c);
                    }
                    out.flush();
                    ins.close();
                    out.close();
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e){
                    e.printStackTrace();
                }
            }

            // 输出到文件 会乱码
            void writeToFile(Blob content) throws SQLException {
                BufferedInputStream in = new BufferedInputStream(content.getBinaryStream());
                BufferedReader reader = null;
                try {
                    reader = new BufferedReader(new InputStreamReader(in));
                    StringBuilder lines = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null){
                        lines.append(line);
                    }
                    OutputStreamWriter pw = new OutputStreamWriter(new FileOutputStream("D:/wanjun/test_blob2.txt"), "UTF-8");
                    pw.write(lines.toString());
                    pw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            // 输出到控制台 应该大部分正常
            void write(Blob blob) throws SQLException {
                byte[] bytes;
                bytes = blob.getBytes(1, (int) blob.length());
                Charset code = getCoder(bytes);
                System.out.println("###file encoding: " + code.toString());
                try {
                    String result = new String(bytes, "GBK");
                    String utf8text = new String(result.getBytes("utf-8"));
                    System.out.println("GBK:" + result);
                    System.out.println("UTF8" + utf8text);
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                }
            }
        },id);
    }

    // 判断文件编码
    // gb2312 --> gbk
    // gb2312 不包括〇
    private static Charset getCoder(byte[] b) {
        try {
            ByteBuffer bb = ByteBuffer.wrap(b);
            CharBuffer cb = CharBuffer.allocate(bb.capacity());

            CharsetDecoder cd1 = Charset.forName("gb2312").newDecoder();

            CoderResult r = cd1.decode(bb, cb, true);
            if (!r.isError()) {
                // gb2312
                return Charset.forName("gb2312");
            } else {
                // utf8
                return Charset.forName("utf-8");
            }
        } catch (Exception e) {
            e.printStackTrace();
            // 大多数是gb2312
            return Charset.forName("gb2312");
        }
    }