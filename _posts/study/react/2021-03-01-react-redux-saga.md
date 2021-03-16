---
layout: post
title:  "redux-saga"
subtitle:   "saga 사용"
date: 2021-03-01 16:29
categories: study
tags: react
comments: true
---

# redux-saga?
    redux-saga는 redux-thunk와 같이 가장 많이 사용되는 라이브러리
    액션을 모니터링하고 있다가, 특정 액션이 발생하면 이에 따라 특정 작업을 하는 방식으로 사용합니다.
    내부적으로 제네레이터 문법을 사용하기 때문에 thunk에서 처리하기 힘든 다양한 작업을 할 수 있음
    비동기 작업을 할 때 기존 요청을 취소할 수 있다
    특정 액션이 발생했을 때 다른 action을 dispatch 하거나 javascript 코드를 실행 할 수 있다
    API 요청이 실패했을 경우 재요청하는 작업을 할 수 있다
    내부적으로 제네레이터 문법을 사용한다
    redux-thunk는 액션 creator를 직접 실행했지만 saga는 직접 실행하지 않고 이벤트 리스너 역할을 함


# 설치
    npm i redux-thunk
    npm i next-redux-saga


# 참고 코드
    // next와 redux를 연결하기 위한 방법
    // redux-saga 연결시 root Component를 withReduxSaga로 감싼다    
    변경 전 : export default wrapper.withRedux(RootProject);
    변경 후 : export default wrapper.withRedux(withReduxSaga(RootProject));


    import { put, all, call, fork, take, takeEvery } from 'redux-saga/effects';
    import axios from "axios";

    function logInAPI() {
        return axios.post('/api/login');
    }
    function logOutAPI() {
        return axios.post('/api/login');
    }
    function addPostAPI() {
        return axios.post('/api/login');
    }
    function* logIn(action) {
        try {
            const result = yield call(logInAPI, action.data);
            yield put({
                type : 'LOG_IN_SUCCESS',
                data : result.data,
            });
        } catch (err) {
            yield put({
                type : 'LOG_IN_FAILURE',
                data : err.response.data,
            });
        }
    }
    function* logOut() {
        try {
            const result = yield call(logOutAPI);
            yield put({
                type : 'LOG_OUT_SUCCESS',
                data : result.data,
            });
        } catch (err) {
            yield put({
                type : 'LOG_OUT_FAILURE',
                data : err.response.data,
            });
        }
    }
    function* addPost(action) {
        try {
            const result = yield call(addPostAPI, action.data);
            yield put({
                type : 'ADD_POST_SUCCESS',
                data : result.data,
            });
        } catch (err) {
            yield put({
                type : 'ADD_POST_FAILURE',
                data : err.response.data,
            });
        }
    }
    function* watchLogIn() {
        //takeEvery : 제네레이터 내의 while문과 같은 기능을 해줌
        //제네레이터 함수를 이벤트리스너처럼 만들어준다
        //while문 사용을 대체할수있음 더 직관적이다
        yield takeEvery('LOG_IN_REQUEST', logIn);
        
    }
    function* watchLogOut() {
        while (true) {
        //제네레이터 함수를 이벤트 리스너로 만들기 위한 방법 while문을 준다
        //yield를 만나면 종료되어 무한반복 하지 않음 실행후 종료
            yield take('LOG_OUT_REQUEST', logOut);
        }
    }
    function* watchAddPost() {
        while (true) {
            yield take('ADD_POST_REQUEST', addPost);
        }
    }
    export default function* rootSaga() {
        yield all([
            fork(watchLogIn),
            fork(watchLogOut),
            fork(watchAddPost),
        ])
    }


# 주의
    제네레이터 함수에서 takeEvery 사용시 발생하는 문제점
    사용자가 실수로 두번 클릭했다고 가정하자 takeEvery는 같은 액션이 동시에 발생하면 무시하지않고 클릭한 횟수만큼 실행한다
    이를 방지하기 위해 takeLatest를 사용했다 
    위 같은 상황일 경우 앞에서 발생한 실행 상태인 이벤트를 무시하고 마지막 이벤트를 실행한다
    



