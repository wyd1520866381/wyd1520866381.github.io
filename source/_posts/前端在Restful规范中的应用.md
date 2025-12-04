---
title: 前端在Restful规范中的应用
tags:
  - Restful
categories:
  - 规范
date: 2025-05-30 11:32:15
---


## 前端在 RESTful 规范中的应用

### 1. RESTful 概述

REST（Representational State Transfer，表述性状态转移）是一种软件架构风格，它定义了一组用于创建 Web 服务的约束。RESTful API 的核心思想是将网络上的所有事物都视为资源，并通过统一的接口进行操作。它基于 HTTP 协议，利用 HTTP 方法（GET、POST、PUT、DELETE 等）来表示对资源的操作，并通过 URL 来定位资源。RESTful API 的设计目标是实现高性能、可伸缩性、简单性和可维护性。

**核心原则：**

- **资源（Resource）**：网络上的所有事物都被抽象为资源，每个资源都有唯一的标识符（URI）。
- **统一接口（Uniform Interface）**：客户端通过统一的 HTTP 方法对资源进行操作。
- **无状态（Stateless）**：服务器不保存客户端的任何会话信息，每次请求都包含所有必要的信息。
- **可缓存（Cacheable）**：客户端可以缓存服务器的响应，提高性能。
- **分层系统（Layered System）**：客户端无法直接连接到最终服务器，而是通过中间服务器进行通信。
- **按需代码（Code-On-Demand，可选）**：服务器可以临时扩展或定制客户端功能，通过下载并执行可执行代码。

### 2. 前端如何遵循 RESTful 规范

在前端开发中，遵循 RESTful 规范主要体现在以下几个方面：

#### 2.1 URL 设计

URL 是资源的唯一标识符，应遵循以下原则：

- **宾语必须是名词，且使用复数**：URL 表示资源，因此应该使用名词的复数形式。例如，获取所有文章使用 `/articles`，而不是 `/getAllArticles`。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- **避免多级 URL，使用查询字符串**：对于资源的过滤、排序等操作，应使用查询字符串，而不是通过多级 URL 来表示。例如，查询已发布的文章使用 `/articles?published=true`，而不是 `/articles/published`。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>

**示例：**

| 操作         | 传统 URI                | RESTful URI    | HTTP 方法 |
| :----------- | :---------------------- | :------------- | :-------- |
| 查询所有员工 | `/employee/list`        | `/employees`   | `GET`     |
| 查询单个员工 | `/employee/list?id=1`   | `/employees/1` | `GET`     |
| 添加员工     | `/employee/add`         | `/employees`   | `POST`    |
| 修改员工     | `/employee/update`      | `/employees`   | `PUT`     |
| 删除员工     | `/employee/delete?id=1` | `/employees/1` | `DELETE`  |

#### 2.2 HTTP 方法

HTTP 方法用于表示对资源的操作，前端在发起请求时应根据操作类型选择正确的 HTTP 方法：

- `GET`：从服务器获取资源。用于查询操作，不应有副作用。
- `POST`：在服务器新建资源。用于创建操作。
- `PUT`：在服务器更新资源（整体更新）。用于完整更新资源。
- `PATCH`：在服务器更新资源（部分更新）。用于部分更新资源。
- `DELETE`：从服务器删除资源。用于删除操作。

#### 2.3 状态码处理

服务器返回的 HTTP 状态码是前端判断请求结果的重要依据。前端应根据不同的状态码进行相应的处理，例如：

- `200 OK`：请求成功。通常用于 `GET`、`PUT`、`PATCH` 请求的成功响应。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `201 Created`：资源创建成功。通常用于 `POST` 请求的成功响应。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `204 No Content`：删除成功，但没有返回内容。通常用于 `DELETE` 请求的成功响应。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `400 Bad Request`：客户端请求错误，服务器不理解请求。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `401 Unauthorized`：用户未认证或认证失败。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `403 Forbidden`：用户已认证但无权限访问。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `404 Not Found`：请求的资源不存在。<mcreference link="https://ruanyifeng.com/blog/2018/10/restful-api-best-practices.html" index="0">0</mcreference>
- `500 Internal Server Error`：服务器内部错误。

#### 2.4 数据格式

RESTful API 通常使用 JSON 作为数据交换格式，因为它易于阅读和解析，并且能直接被 JavaScript 读取。<mcreference link="https://blog.csdn.net/zzvar/article/details/118164133" index="1">1</mcreference>

### 3. UmiJS 框架中处理 RESTful API

这里以 UmiJS 框架为例，UmiJS 是一个企业级前端应用框架（React）。在 UmiJS 中处理 RESTful API，通常会结合 `umi-request` 或 `fetch` API，并配合 `dva` 或 `umi-initial-state` 等数据流方案。

#### 3.1 请求封装

为了更好地管理 API 请求，通常会进行统一的请求封装，例如设置基础 URL、请求头、错误处理等。

```javascript
// src/utils/request.js
import { extend } from "umi-request";
import { notification } from "antd";

const errorHandler = (error) => {
  const { response } = error;
  if (response && response.status) {
    const errorText = `请求错误 ${response.status}: ${response.url}`;
    notification.error({
      message: `请求错误 ${response.status}`,
      description: errorText,
    });
  } else if (!response) {
    notification.error({
      message: "网络异常",
      description: "您的网络发生异常，无法连接服务器",
    });
  }
  return response;
};

const request = extend({
  errorHandler, // 默认错误处理
  // prefix: '/api', // 如果后端API有统一前缀，可以在这里设置
});

export default request;
```

#### 3.2 API 服务定义

将不同模块的 API 请求定义为独立的 Service 文件，便于管理和维护。

```javascript
// src/services/user.js
import request from "@/utils/request";

export async function queryUsers(params) {
  return request("/api/users", {
    method: "GET",
    params,
  });
}

export async function createUser(data) {
  return request("/api/users", {
    method: "POST",
    data,
  });
}

export async function updateUser(id, data) {
  return request(`/api/users/${id}`, {
    method: "PUT",
    data,
  });
}

export async function deleteUser(id) {
  return request(`/api/users/${id}`, {
    method: "DELETE",
  });
}
```

#### 3.3 在组件中使用

在 UmiJS 的页面或组件中，可以通过 `useRequest` (ahooks) 或 `dva` 的 `models` 来调用 API 服务。

**使用 `ahooks` 的 `useRequest`：**

```javascript
// src/pages/users/index.jsx
import React from "react";
import { useRequest } from "ahooks";
import { Table, Button, Popconfirm, message } from "antd";
import { queryUsers, deleteUser } from "@/services/user";

const UserList = () => {
  const { data, loading, run } = useRequest(queryUsers);

  const handleDelete = async (id) => {
    try {
      await deleteUser(id);
      message.success("删除成功");
      run(); // 重新加载数据
    } catch (error) {
      message.error("删除失败");
    }
  };

  const columns = [
    {
      title: "ID",
      dataIndex: "id",
      key: "id",
    },
    {
      title: "姓名",
      dataIndex: "name",
      key: "name",
    },
    {
      title: "操作",
      key: "action",
      render: (_, record) => (
        <Popconfirm
          title="确定删除吗?"
          onConfirm={() => handleDelete(record.id)}
        >
          <Button type="link" danger>
            删除
          </Button>
        </Popconfirm>
      ),
    },
  ];

  return (
    <Table
      dataSource={data?.list}
      columns={columns}
      loading={loading}
      rowKey="id"
    />
  );
};

export default UserList;
```

**使用 `dva` 的 `models`：**

```javascript
// src/models/user.js
import {
  queryUsers,
  createUser,
  updateUser,
  deleteUser,
} from "@/services/user";

const UserModel = {
  namespace: "user",
  state: {
    list: [],
    total: 0,
  },
  effects: {
    *fetch({ payload }, { call, put }) {
      const response = yield call(queryUsers, payload);
      if (response) {
        yield put({
          type: "save",
          payload: response,
        });
      }
    },
    *add({ payload, callback }, { call, put }) {
      const response = yield call(createUser, payload);
      if (response && callback) {
        callback();
      }
    },
    *update({ payload, callback }, { call, put }) {
      const { id, ...rest } = payload;
      const response = yield call(updateUser, id, rest);
      if (response && callback) {
        callback();
      }
    },
    *remove({ payload, callback }, { call, put }) {
      const response = yield call(deleteUser, payload);
      if (response && callback) {
        callback();
      }
    },
  },
  reducers: {
    save(state, action) {
      return {
        ...state,
        list: action.payload.list,
        total: action.payload.total,
      };
    },
  },
};

export default UserModel;
```

在组件中连接 `dva` model：

```javascript
// src/pages/users/index.jsx (使用 dva)
import React, { useEffect } from "react";
import { connect } from "umi";
import { Table, Button, Popconfirm, message } from "antd";

const UserList = ({ dispatch, user, loading }) => {
  useEffect(() => {
    dispatch({
      type: "user/fetch",
    });
  }, [dispatch]);

  const handleDelete = async (id) => {
    try {
      await dispatch({
        type: "user/remove",
        payload: id,
        callback: () => {
          message.success("删除成功");
          dispatch({
            type: "user/fetch",
          });
        },
      });
    } catch (error) {
      message.error("删除失败");
    }
  };

  const columns = [
    {
      title: "ID",
      dataIndex: "id",
      key: "id",
    },
    {
      title: "姓名",
      dataIndex: "name",
      key: "name",
    },
    {
      title: "操作",
      key: "action",
      render: (_, record) => (
        <Popconfirm
          title="确定删除吗?"
          onConfirm={() => handleDelete(record.id)}
        >
          <Button type="link" danger>
            删除
          </Button>
        </Popconfirm>
      ),
    },
  ];

  return (
    <Table
      dataSource={user.list}
      columns={columns}
      loading={loading.effects["user/fetch"]}
      rowKey="id"
    />
  );
};

export default connect(({ user, loading }) => ({
  user,
  loading,
}))(UserList);
```

### 4. 总结

遵循 RESTful 规范可以使前端与后端之间的通信更加清晰、高效和可维护。通过统一的 URL 设计、HTTP 方法和状态码处理，前端可以更好地理解和操作后端资源。在 UmiJS 这样的现代前端框架中，结合请求封装和数据流管理，可以优雅地实现 RESTful API 的调用和数据处理，从而提升开发效率和应用性能。
