---
layout: post
title: SAP B1 9.0版本数据库备份移动导致JCO无法连接
description: "SAP B1 9.0版本数据库备份移动造成的JCO无法连接问题"
modified: 2015-02-04
category: articles
tags: [SAP]
comments: true
share: true
---

##SAP B1 9.0版本数据库备份移动导致JCO无法连接

###连接代码 

测试连接代码如下：

		public static void main(String[] args) 
	    {	

	         ICompany company;
	         IDocuments pedido;     
	         SBOCOMUtil util = new SBOCOMUtil();
	         company = util.newCompany();
	         try 
	         {
	              company.setServer( "WIN-123" );
	              company.setCompanyDB( "Test" );
	              company.setUserName( "test" );
	              company.setPassword( "123" );
	              company.setLanguage(com.sap.smb.sbo.api.SBOCOMConstants.BoSuppLangs_ln_English);
	              company.setDbUserName("sa");
	              company.setDbPassword("123");
	              company.setUseTrusted( new Boolean(false) ); 
	              int     result = company.connect();
	              System.out.println("Company: " + company.getCompanyName());
	              // analize connection result
	              if ( result != 0 ) 
	              {
	                   System.out.println("Connection error: " + result);
	              }
	              else 
	              {
	                   System.out.println("Connection success,     company     name: "     + company.getCompanyName() );
	                   pedido = util.newDocuments(company,new Integer(22));
	                   if(pedido.getByKey(new Integer(286)))
	                   {
	                        System.out.println("Pedido recuperado." + pedido.getDocEntry());
	                   }
	                   else
	                   {
	                        System.out.println("Error al reuperar" + company.getLastErrorCode() + " - " + company.getLastErrorDescription());
	                   }
	              }
	         }
	         catch(SBOCOMException ex)
	         {
	              System.out.println(ex.getStackTraceString());
	         }
	         finally 
	         {
	              company.disconnect();
	         }
	    }

正常情况下result返回结果应该为0，代表正常连接，否则代表连接异常。

当数据库从其他机器备份恢复到本机的时候，再进行连接就会返回result=-8073。
Error Code: -8037 Failed to connect or logon to SLD, please check connection parameters and configure file (or)Extension Property not found in SLD.


相关解决方案： 
打开相应的DB数据库，使用 `select DKeyID from CINF`查看该项的值，如果有值，代表需要重新将其赋为空，执行SQL `update CINF set DkeyId = null`即可。


详细可参考SCN相关问题ticket  [-8037 Moving DB from one SAP B1 9.0 to another SAP B1 9.0 Server. Same DB.](http://scn.sap.com/thread/3378717)