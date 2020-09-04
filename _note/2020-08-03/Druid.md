---
layout: note
title: Druid 的总结
author: 陈继淼
category: 2020暑假第四周
date: 2020-08-09
---



#Druid 

Apache Druid 是一个分布式内存实时分析系统，用于解决如何在大规模数据集下进行快速的、交互式的查询和分析的问题。Apache Druid 由 Metamarkets 公司（一家为在线媒体或广告公司提供数据分析服务的公司）开发，在2019年春季被捐献给 Apache 软件基金会。

##Apache Druid 具有以下特点：

* 亚秒级 OLAP 查询，包括多维过滤、Ad-hoc 的属性分组、快速聚合数据等等。
* 实时的数据消费，真正做到数据摄入实时、查询结果实时。
* 高效的多租户能力，最高可以做到几千用户同时在线查询。
* 扩展性强，支持 PB 级数据、千亿级事件快速处理，支持每秒数千查询并发。
* 极高的高可用保障，支持滚动升级。

##配置文件


	url:jdbc:mysql://localhost:3306/dragoon_v25_masterdb
	driverClassName:com.mysql.jdbc.Driver
	username:root
	password:aaaaaaaa
	     
	filters:stat
	 
	maxActive:20
	initialSize:1
	maxWait:60000
	minIdle:10
	#maxIdle:15
	 
	timeBetweenEvictionRunsMillis:60000
	minEvictableIdleTimeMillis:300000
	 
	validationQuery:SELECT 'x'
	testWhileIdle:true
	testOnBorrow:false
	testOnReturn:false
	#poolPreparedStatements:true
	maxOpenPreparedStatements:20



##spring配置文件

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
			http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
			http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">
		<!-- 给web使用的spring文件 -->
		<bean id="propertyConfigurer"
			class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
			<property name="locations">
				<list>
					<value>/WEB-INF/classes/dbconfig.properties</value>
				</list>
			</property>
		</bean>
		<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
			destroy-method="close">
			<property name="url" value="${url}" />
			<property name="username" value="${username}" />
			<property name="password" value="${password}" />
			<property name="driverClassName" value="${driverClassName}" />
			<property name="filters" value="${filters}" />
	 
			<property name="maxActive" value="${maxActive}" />
			<property name="initialSize" value="${initialSize}" />
			<property name="maxWait" value="${maxWait}" />
			<property name="minIdle" value="${minIdle}" />
	 
			<property name="timeBetweenEvictionRunsMillis" value="${timeBetweenEvictionRunsMillis}" />
			<property name="minEvictableIdleTimeMillis" value="${minEvictableIdleTimeMillis}" />
	 
			<property name="validationQuery" value="${validationQuery}" />
			<property name="testWhileIdle" value="${testWhileIdle}" />
			<property name="testOnBorrow" value="${testOnBorrow}" />
			<property name="testOnReturn" value="${testOnReturn}" />
			<property name="maxOpenPreparedStatements"
				value="${maxOpenPreparedStatements}" />
			
		</bean>
		
		<bean id="dataSourceDbcp" class="org.apache.commons.dbcp.BasicDataSource"
			destroy-method="close">
	 
			<property name="driverClassName" value="${driverClassName}" />
			<property name="url" value="${url}" />
			<property name="username" value="${username}" />
			<property name="password" value="${password}" />
			
			<property name="maxActive" value="${maxActive}" />
			<property name="minIdle" value="${minIdle}" />
			<property name="maxWait" value="${maxWait}" />
			<property name="defaultAutoCommit" value="true" />
			
			<property name="timeBetweenEvictionRunsMillis" value="${timeBetweenEvictionRunsMillis}" />
			<property name="minEvictableIdleTimeMillis" value="${minEvictableIdleTimeMillis}" />
			
			<property name="validationQuery" value="${validationQuery}" />
			<property name="testWhileIdle" value="${testWhileIdle}" />
			<property name="testOnBorrow" value="${testOnBorrow}" />
			<property name="testOnReturn" value="${testOnReturn}" />
			<property name="maxOpenPreparedStatements"
				value="${maxOpenPreparedStatements}" />
			
		</bean>
	 
		
		<!-- jdbcTemplate -->
		<bean id="jdbc" class="org.springframework.jdbc.core.JdbcTemplate">
			<property name="dataSource">
				<ref bean="dataSource" />
			</property>
		</bean>
	 
		<bean id="SpringTableOperatorBean" class="com.whyun.druid.model.TableOperator"
			scope="prototype">
			<property name="dataSource">
				<ref bean="dataSource" />
			</property>
		</bean>
		
	</beans>


其中第一个bean中给出的配置文件/WEB-INF/classes/dbconfig.properties就是第1节中给出的配置文件。我这里还特地给出dbcp的spring配置项，目的就是将两者进行对比，方便大家进行迁移。这里没有使用JdbcTemplate，所以jdbc那个bean没有使用到。下面给出com.whyun.druid.model.TableOperator类的代码。


	package com.whyun.druid.model;
	 
	import java.sql.Connection;
	import java.sql.PreparedStatement;
	import java.sql.SQLException;
	import java.sql.Statement;
	 
	import javax.sql.DataSource;
	 
	public class TableOperator {
		private DataSource dataSource;
	    public void setDataSource(DataSource dataSource) {
			this.dataSource = dataSource;
		}
	 
		private static final int COUNT = 800;    
	 
	    public TableOperator() {
			
		}
	 
		public void tearDown() throws Exception {
	        try {
	            dropTable();
	        } catch (SQLException e) {
	            e.printStackTrace();
	        }       
	    }
	 
	    public void insert() throws Exception {
	        
	        StringBuffer ddl = new StringBuffer();
	        ddl.append("INSERT INTO t_big (");
	        for (int i = 0; i < COUNT; ++i) {
	            if (i != 0) {
	                ddl.append(", ");
	            }
	            ddl.append("F" + i);
	        }
	        ddl.append(") VALUES (");
	        for (int i = 0; i < COUNT; ++i) {
	            if (i != 0) {
	                ddl.append(", ");
	            }
	            ddl.append("?");
	        }
	        ddl.append(")");
	 
	        Connection conn = dataSource.getConnection();
	 
	//        System.out.println(ddl.toString());
	 
	        PreparedStatement stmt = conn.prepareStatement(ddl.toString());
	 
	        for (int i = 0; i < COUNT; ++i) {
	            stmt.setInt(i + 1, i);
	        }
	        stmt.execute();
	        stmt.close();
	 
	        conn.close();
	    }
	 
	    private void dropTable() throws SQLException {
	 
	    	Connection conn = dataSource.getConnection();
	 
	        Statement stmt = conn.createStatement();
	        stmt.execute("DROP TABLE t_big");
	        stmt.close();
	 
	        conn.close();
	    }
	 
	    public void createTable() throws SQLException {
	        StringBuffer ddl = new StringBuffer();
	        ddl.append("CREATE TABLE t_big (FID INT AUTO_INCREMENT PRIMARY KEY ");
	        for (int i = 0; i < COUNT; ++i) {
	            ddl.append(", ");
	            ddl.append("F" + i);
	            ddl.append(" BIGINT NULL");
	        }
	        ddl.append(")");
	 
	        Connection conn = dataSource.getConnection();
	 
	        Statement stmt = conn.createStatement();
	        stmt.execute(ddl.toString());
	        stmt.close();
	 
	        conn.close();
	    }
	}



注意：在使用的时候，通过获取完Connection对象，在使用完之后，要将其close掉，这样其实是将用完的连接放入到连接池中，如果你不close的话，会造成连接泄露。

然后我们写一个servlet来测试他.

	package com.whyun.druid.servelt;
	 
	import java.io.IOException;
	import java.io.PrintWriter;
	import java.sql.SQLException;
	 
	import javax.servlet.ServletContext;
	import javax.servlet.ServletException;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	 
	import org.springframework.web.context.WebApplicationContext;
	import org.springframework.web.context.support.WebApplicationContextUtils;
	 
	import com.whyun.druid.model.TableOperator;
	 
	public class TestServlet extends HttpServlet {
		private TableOperator operator;
		
	 
		@Override
		public void init() throws ServletException {
			
			super.init();
			 ServletContext servletContext = this.getServletContext();   
	         
	         WebApplicationContext ctx
	         	= WebApplicationContextUtils.getWebApplicationContext(servletContext);
	         operator = (TableOperator)ctx.getBean("SpringTableOperatorBean");
		}
	 
		/**
		 * The doGet method of the servlet. <br>
		 *
		 * This method is called when a form has its tag value method equals to get.
		 * 
		 * @param request the request send by the client to the server
		 * @param response the response send by the server to the client
		 * @throws ServletException if an error occurred
		 * @throws IOException if an error occurred
		 */
		public void doGet(HttpServletRequest request, HttpServletResponse response)
				throws ServletException, IOException {
	 
			response.setContentType("text/html");
			PrintWriter out = response.getWriter();
	 
			boolean createResult = false;
			boolean insertResult = false;
			boolean dropResult = false;
			
			try {
				operator.createTable();
				createResult = true;
			} catch (SQLException e) {
				e.printStackTrace();
			}
			if (createResult) {
				try {
					operator.insert();
					insertResult = true;
				} catch (Exception e) {
					e.printStackTrace();
				}
				try {
					operator.tearDown();
					dropResult = true;
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
			
			
			out.println("{'createResult':"+createResult+",'insertResult':"
					+insertResult+",'dropResult':"+dropResult+"}");
			
			out.flush();
			out.close();
		}
	 
	}

