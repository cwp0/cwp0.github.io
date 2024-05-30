---
title: react技巧
tags:
  - 移动端
  - 前端
  - react
  - react-native
categories:
  - react
keywords:
  - react
description: 写代码的时候对react-native使用的一些技巧
abbrlink: 56292
date: 2023-10-07 15:03:23
updated: 2023-10-07 15:30:23
top_img: https://s2.loli.net/2024/02/06/lhsVBAGIouNxRUa.png
comments:
cover: https://s2.loli.net/2024/02/06/u1DMChx86N93wPR.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---

> 主要是对开源项目中react-native的一些使用技巧进行总结。具体项目见：[casdoor-app](https://github.com/cwp0/casdoor-app)

> 后面如果做react的话可能也会做一些总结，到时候再加。

# react-native

## React Context

用于在不同的组件中或者 `js` 文件中共享同一个数据。

使用方法：

例如要共享 `userInfo`

1. 在应用程序中创建一个 `React Context`，用于共享 `userInfo`。

   `UserContext.js`

    ```jsx
    import React from "react";
    
    const UserContext = React.createContext();
    export const UserProvider = UserContext.Provider;
    export const UserConsumer = UserContext.Consumer;
    export default UserContext;
    ```


1. 在 `App.js` 中使用 **`UserProvider`** 包裹应用程序

   注意这里要设置需要共享的数据 `value={{userInfo, setUserInfo}}`

    ```jsx
    import * as React from "react";
    import {PaperProvider} from "react-native-paper";
    import NavigationBar from "./NavigationBar";
    import {NavigationContainer} from "@react-navigation/native";
    import Header from "./Header";
    import {UserProvider} from "./UserContext";
    
    const App = () => {
      const [userInfo, setUserInfo] = React.useState(null);
      return (
        <UserProvider value={{userInfo, setUserInfo}} >
          <NavigationContainer>
            <PaperProvider>
              <Header />
              <NavigationBar />
            </PaperProvider>
          </NavigationContainer>
        </UserProvider>
      );
    };
    export default App;
    ```


1. 在其他组件中，通过导入 `UserContext.js` 进行使用共享的数据。

   注意：声明新的 `React.useContext`的时候需要用花括号。

   `Header.js`

    ```jsx
    import * as React from "react";
    import {Appbar, Avatar, Button, Menu, Text} from "react-native-paper";
    import UserContext from "./UserContext";
    import {View} from "react-native";
    import CasdoorLoginPage, {CasdoorLogout} from "./CasdoorLoginPage";
    
    const Header = () => {
      const {userInfo, setUserInfo} = React.useContext(UserContext);
      const [showLoginPage, setShowLoginPage] = React.useState(false);
      const [menuVisible, setMenuVisible] = React.useState(false);
      const openMenu = () => setMenuVisible(true);
      const closeMenu = () => setMenuVisible(false);
      const handleMenuLogoutClicked = () => {
        handleCasdoorLogout();
        closeMenu();
      };
    
      const handleCasdoorLogin = () => {
        setShowLoginPage(true);
      };
      const handleCasdoorLogout = () => {
        CasdoorLogout();
        setUserInfo(null);
      };
      const handleHideLoginPage = () => {
        setShowLoginPage(false);
      };
      return (
        <View>
          <Appbar.Header style={{height: 40}}>
            <Appbar.Content title="Casdoor" />
            <Menu
              visible={menuVisible}
              anchor={
                <Button
                  style={{marginRight: 10, backgroundColor: "transparent", height: 40}}
                  onPress={userInfo === null ? handleCasdoorLogin : openMenu}
                >
                  {
                    userInfo === null ?
                      null :
                      <Avatar.Image
                        size={32}
                        source={{uri: userInfo.avatar}}
                        style={{marginRight: 10, backgroundColor: "transparent"}}
                      />
                  }
                  <Text style={{marginRight: 10}} variant="titleMedium">
                    {userInfo === null ? "Login" : userInfo.name}
                  </Text>
                </Button>
              }
              onDismiss={closeMenu}
            >
              <Menu.Item onPress={() => handleMenuLogoutClicked()} title="Logout" />
            </Menu>
          </Appbar.Header>
          {showLoginPage && <CasdoorLoginPage onWebviewClose={handleHideLoginPage} />}
        </View>
      );
    };
    
    export default Header;
    ```


## 子组件使用父组件的函数等参数

在父组件中：

定义好相关函数及变量，将参数传递给子组件。

```jsx
const [placeholder, setPlaceholder] = React.useState("");

const closeEditAccountModal = () => {
    setShowEditAccountModal(false);
};

const onAccountEdit = (accountDescp) => {
    const accountToEdit = accountList.find(account => account.getEditStatus() === true);
    if (accountToEdit) {
      accountToEdit.setTitle(accountDescp);
    }
    setPlaceholder("");
    closeEditAccountModal();
}

<EditAccountDetails onClose={closeEditAccountModal} onEdit={onAccountEdit} placeholder={placeholder} />
```

在子组件中：

添加相关代码，将父组件的函数等参数接收过来，然后就可以使用父组件的相关参数了。

```jsx
import React, {useState} from "react";
import {Text, TextInput, View} from "react-native";
import {Button, IconButton} from "react-native-paper";
import PropTypes from "prop-types";

export default function EnterAccountDetails({onClose, onEdit, placeholder}) {
  EnterAccountDetails.propTypes = {
    onClose: PropTypes.func.isRequired,
    onEdit: PropTypes.func.isRequired,
    placeholder: PropTypes.string.isRequired,
  };

  const [description, setDescription] = useState("");

  const handleConfirm = () => {
    onEdit(description);
  };
  return (
    <View style={{flex: 1, justifyContent: "center", alignItems: "center"}}>
      <Text style={{fontSize: 24, marginBottom: 5}}>Enter new description</Text>
      <View style={{flexDirection: "row", alignItems: "center"}}>
        <IconButton icon="account-details" size={35} />
        <TextInput
          placeholder={placeholder}
          value={description}
          onChangeText={(text) => setDescription(text)}
          style={{borderWidth: 3, borderColor: "white", margin: 10, width: 230, height: 50, borderRadius: 5, fontSize: 18, color: "gray", paddingLeft: 10}}
        />
      </View>
      <Button
        style={{
          backgroundColor: "#E6DFF3",
          borderRadius: 5,
          margin: 10,
          alignItems: "center",
          position: "absolute",
          top: 160,
          width: 300,
        }}
        onPress={handleConfirm}
      >
        <Text style={{fontSize: 18, width: 280}}>Confirm</Text>
      </Button>
      <IconButton icon={"close"} size={30} onPress={onClose} style={{position: "absolute", top: 5, right: 5}} />
    </View>
  );
}
```

## 父组件使用自组件的函数等参数

在子组件中：

将父组件要使用的函数导出：

```jsx
import React, {useEffect} from "react";
import {WebView} from "react-native-webview";
import {View} from "react-native";
import {Portal} from "react-native-paper";
import CasdoorSdkConfig from "./CasdoorSdkConfig";
import SDK from "casdoor-react-native-sdk";
import UserContext from "./UserContext";
import PropTypes from "prop-types";
// import {LogBox} from "react-native";
// LogBox.ignoreAllLogs();
const sdk = new SDK(CasdoorSdkConfig);

const CasdoorLoginPage = ({onWebviewClose}) => {
  CasdoorLoginPage.propTypes = {
    onWebviewClose: PropTypes.func.isRequired,
  };
  const [casdoorLoginURL, setCasdoorLoginURL] = React.useState("");
  const {setUserInfo} = React.useContext(UserContext);
  const getCasdoorSignInUrl = async() => {
    const signinUrl = await sdk.getSigninUrl();
    setCasdoorLoginURL(signinUrl);
  };

  useEffect(() => {
    getCasdoorSignInUrl();
  }, []);

  const onNavigationStateChange = async(navState) => {
    if (navState.url.startsWith(CasdoorSdkConfig.redirectPath)) {
      onWebviewClose();
      const token = await sdk.getAccessToken(navState.url);
      const userInfo = sdk.JwtDecode(token);
      setUserInfo(userInfo);
    }
  };

  return (
    <Portal>
      <View style={{flex: 1}}>
        {casdoorLoginURL !== "" && (
          <WebView
            source={{uri: casdoorLoginURL}}
            onNavigationStateChange={onNavigationStateChange}
            style={{flex: 1}}
            mixedContentMode="always"
            javaScriptEnabled={true}
          />
        )}
      </View>
    </Portal>
  );
};

export const CasdoorLogout = () => {
  sdk.clearState();
};
export default CasdoorLoginPage;
```

在父组件中：

导入子组件及其导出的函数，注意导出的方式，导出的组件直接 `import` ，导出的组件的函数要使用花括号来包裹。
