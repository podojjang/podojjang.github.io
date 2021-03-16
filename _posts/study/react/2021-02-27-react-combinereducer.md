---
layout: post
title:  "redux reducer 결합"
subtitle:   "combineReducer"
date: 2021-02-27 23:44
categories: study
tags: react
comments: true
---

# 개요
    redux reducer 결합


# combineReducer?
    reducer의 액션이 늘어남에 따라 액션을 처리하는 함수도 늘어나
    모듈의 길이가 지나치게 길어진다 그래서 적절하게 모듈을 나누는 방법이 필요하다
    이 때 combineReducer를 사용하는데 별도로 분리한 모듈을 reducer에 하나로 결합시켜주는 기능이 있다
    reducer가 객체가 아니라 함수이기 때문에 combineReducers를 메소드를 사용해서 결합이 쉽도록 한다
    
# index reducer
    import { HYDRATE } from "next-redux-wrapper";
    import user from './user';
    import {combineReducers} from "redux";

    
    const rootReducer = combineReducers({
        index : (state = {}, action) => {
            switch (action.type) {
                case HYDRATE :
                    console.log('HYDRATE ACTION');
                    return {
                        ...state,
                        ...action.payload,
                    }
                default :
                    return state;
                }
            },
        user, //index reducer에 결합 되었다
    });
    
    export default rootReducer;

# user reducer
    export const initialState = {
        isLoggedIn : false,
        me : null,
        signUpData : {},
        loginData : {},
    };

    export const loginAction = (data) => {
        return {
            type : 'LOG_IN',
            data,
        }
    }
    export const logoutAction = () => {
        return {
            type : 'LOG_OUT',
        }
    }

    const reducer = (state = initialState, action) => {
        switch (action.type) {
            case ('LOG_IN') :
                return {
                    ...state,
                    isLoggedIn: true,
                    me : action.data,
                }
            case 'LOG_OUT' :
                return {
                    ...state,
                    isLoggedIn: false,
                    me : null,
                }
            default :
                return state;
        }
    };
    export default reducer;