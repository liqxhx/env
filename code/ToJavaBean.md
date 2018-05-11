```
public static <T> T toJavaBean(Map dbData, Class<T> type) {
        T target = null;
        try {
            target = type.newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        if(target == null){
            return target;
        }

        Field[] fields = type.getDeclaredFields();
        for(Object key: dbData.keySet()) {
            for(Field f: fields){
                if(f.getName().equalsIgnoreCase(String.valueOf(key).replaceAll("_",""))){
                    f.setAccessible(true);
                    if(Date.class.isAssignableFrom(f.getType())){

                        if(dbData.get(key).getClass() == String.class) {
                            try {
                                f.set(target, new DateStringConverter("yyyy-MM-dd HH:mm:ss").fromString(dbData.get(key).toString()));
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }else if(dbData.get(key).getClass() == Date.class){
                            try {
                                f.set(target, dbData.get(key));
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }
                    } else {
                        try {
                            f.set(target, dbData.get(key));
                        } catch (Exception e) {
                            e.printStackTrace();
                        }
                    }
                }
            }

        }

        return target;
    }
```
