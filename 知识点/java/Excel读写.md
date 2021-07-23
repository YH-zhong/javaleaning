依赖包
        <!-- 引入poi，解析workbook视图 -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.16</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.14</version>
        </dependency>
        <!-- 处理excel和上面功能是一样的-->
        <dependency>
            <groupId>net.sourceforge.jexcelapi</groupId>
            <artifactId>jxl</artifactId>
            <version>2.6.10</version>
        </dependency>
        
        
        
        
        workBook = getWorkbok(finalXlsxFile);
            // sheet 对应一个工作页
            Sheet sheet = workBook.getSheetAt(0);
            // 第一行从0开始算
            rowNumber = sheet.getLastRowNum();
            // 创建文件输出流，输出电子表格：这个必须有，否则你在sheet上做的任何操作都不会有效
            out =  new FileOutputStream(finalXlsxPath);
            workBook.write(out);


            // 得到要插入的每一条记录
            for (int i = 0; i < dataList.size(); i++) {
                List list = dataList.get(i);
                // 创建一行：从第二行开始，跳过属性列
                Row row = sheet.createRow(rowNumber+1+i);
                for (int k = 0; k < list.size(); k++) {
                    // 在一行内循环
                    Cell cell = row.createCell(k);
                    cell.setCellValue((String) list.get(k));
                    logger.info("第"+ (rowNumber+2+i) + "行" + "第"+ (k+1) + "列数据插入");
                }
            }
            // 创建文件输出流，准备输出电子表格：这个必须有，否则你在sheet上做的任何操作都不会有效
            out =  new FileOutputStream(finalXlsxPath);
            workBook.write(out);
        } catch (Exception e) {
            e.printStackTrace();
        } finally{
            try {
                if(out != null){
                    out.flush();
                    out.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 判断Excel的版本,获取Workbook
     * @param
     * @param
     * @return
     * @throws IOException
     */
    public static Workbook getWorkbok(File file) throws IOException{
        Workbook wb = null;
        FileInputStream in = new FileInputStream(file);
        if(file.getName().endsWith(EXCEL_XLS)){     //Excel&nbsp;2003
            wb = new HSSFWorkbook(in);
        }else if(file.getName().endsWith(EXCEL_XLSX)){    // Excel 2007/2010
            wb = new XSSFWorkbook(in);
        }
        return wb;
    }
