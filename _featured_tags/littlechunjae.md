---
title: LittleChunjae
layout: page
slug: littlechunjae
category: project
menu: false
order: 2
---

### 소개

기존 리틀천재 홈페이지를 리뉴얼하는 작업을 진행했습니다<br/>
사용자 화면(유저화면, CRUD 형식의 게시판)과 관리자 페이지 등을 작업했습니다 <br/>
게시판은 다중 이미지를 처리할 수 있도록 개발하였고  AJAX를 적용하였습니다<br/>
작업과정에서 성능개선을 위한 쿼리최적화 작업도 진행하였습니다.<br/>
쿠키와 세션의 사용에 능숙해졌습니다<br/>

---
### Skill
Language : Java<br/>
Web Framework : Spring<br/>
DataBase : MSSQL<br/>
ORM : Mybatis<br/>

---
### 관리자화면

![https://podojjang.github.io/assets/img/little/dddd(2).PNG](https://podojjang.github.io/assets/img/little/dddd(2).PNG)
<br/>


### 사용자화면

![https://podojjang.github.io/assets/img/little/dddd(5).PNG](https://podojjang.github.io/assets/img/little/dddd(5).PNG)
<br/>


### 쿠키를 통한 로그인처리
	if(cookie!=null){
		if(name==null){
			$.ajax({
				url:"/login/login.do",
				type:"POST",
				success: function(result) {
					console.log('result', result)
					var data = {};
					setCookie("name",result[0].name, 1);
					location.reload();
			}
			});
		}
	}
			
### navigator.platform객체를 이용한 반응형 처리
	if (navigator.platform) {
		var mall_url = "https://mall.chunjae.co.kr/#/shop/shopSub/0101?pageNum=1&sort=newest&searchText=&saleyn="; //pc
			if (0 > filter.indexOf(navigator.platform.toLowerCase())) {
				mall_url = "https://mmall.chunjae.co.kr/#/shop/shopSub/0101?pageNum=1&sort=newest&searchText=&saleyn="; //mobile
			}
			var win = window.open(mall_url, '_blank');
			win.focus();
		}

### 비동기방식 게시판 처리
	$.ajax({
		type : 'POST',
		url : '/menu/listAjax.json',
		dataType : 'json',
		success : function(result) {
	
			var dol = result.dol;
			var str = "";
			$.each(dol, function(key, value) {
				str += '<li><a href="/series/read.do?id=' + value.ID + '&menuNum=1">' + value.MENUTITLE + '</a></li>';
			})
			$("#dol_m").html(str);
			$("#dol").html(str);

			var big = result.big;
			str = "";
			$.each(big, function(key, value) {
				str += '<li><a href="/series/read.do?id=' + value.ID + '&menuNum=1">' + value.MENUTITLE + '</a></li>';
			})
			$("#big_m").html(str);
			$("#big").html(str);

			var book = result.book;
			str = "";
			$.each(book, function(key, value) {
				str += '<li><a href="/series/read.do?id=' + value.ID + '&menuNum=2">' + value.MENUTITLE + '</a></li>';
			})
		$("#book_m").html(str);
		$("#book").html(str);

		var edu = result.edu;
		str = "";
		$.each(edu, function(key, value) {
			str += '<li><a href="/series/read.do?id=' + value.ID + '&menuNum=2">' + value.MENUTITLE + '</a></li>';
		})
			$("#book_m").append(str);
			$("#edu").html(str);
		},
		error : function(request, status, error) {
			return false;
		}
	});
