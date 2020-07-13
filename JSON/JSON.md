```JAVA
@Test
public void JsonCreate() {
    DataDao dataDao = new DataDaoImpl();
    BioVolt bioVolt = new BioVolt();
    try {
        bioVolt = dataDao.select_dataBydataNum("01", 1);
        System.out.println(new JSONObject(bioVolt));
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
@Test
    public void JsonCreate2() {
        DataDao dataDao = new DataDaoImpl();
        BioVolt bioVolt = new BioVolt();
        try {
            bioVolt = dataDao.select_dataBydataNum("01", 1);
            GsonBuilder gsonBuilder = new GsonBuilder();
            Gson gson =gsonBuilder.create();
            System.out.println(gson.toJson(bioVolt));
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }

public static void main(String[] args) {
        File file = new File(ReadJsonSample.class.getResource("/aqi-beijing.json").getFile());
        try {
            String content = FileUtils.readFileToString(file);
            JSONObject jsonObject = new JSONObject(content);
            System.out.println("日期"+jsonObject.getString(""));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

