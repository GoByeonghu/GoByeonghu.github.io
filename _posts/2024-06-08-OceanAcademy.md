---
layout: post
title: "[프로젝트] 바다서원"
subtitle: 개인 라이브 강의 플랫폼
categories: 
  - OceanAcademy
tags: [webrtc]
---

<div align="center">
<a href="https://refine.dev/">
    <img alt="refine logo" src="{{site.url}}/PostImages/2024-10-22-OceanAcademy/banner.png">
</a>

<br/>
<br/>

<div align="center">
    <a href="https://github.com/100-hours-a-week/5-nemo-oceanAcademy-be">Github</a> |
    <a href="https://refine.dev/docs/">Documentation</a> |
    <a href="https://gobyeonghu.github.io/oceanacademy/2024/06/08/OceanAcademy.html">Blog</a>
</div>
</div>

<br/>
<br/>

<div align="center">
본 프로젝트는 오프라인 교육의 실시간 소통 장점과 온라인 플랫폼의 접근성을 결합
한 라이브 강의 플랫폼을 개발하는 것을 목표로 한다. 또한, WebRTC 기반의 미디어 전
송 최적화와 클라우드 인프라를 도입하여 확장성을 개선하고, 동시 접속자가 많은 상
황에서도 안정적인 스트리밍 환경을 제공한다. 이러한 방식으로 학습자와 강사 간의
상호작용을 극대화하고, 플랫폼의 신뢰성과 사용자 경험을 향상시키고자 한다.
<br>궁극적으로, <strong>본 프로젝트는 학습자들이 언제 어디서나 원활한 환경에서 교육에 집중
할 수 있도록 하여 더 나은 학습 경험을 제공하는 것을 지향한다.</strong>
<br />
<br />

</div>

<div align="center">

[![Gmail](https://img.shields.io/badge/Email-ktb.nemo%40gmail.com-blue.svg)](mailto:ktb.nemo@gmail.com)


</div>

<br/>

## What is OceanAcademy

본 프로젝트는 오프라인 교육의 실시간 소통 장점과 온라인 플랫폼의 접근성을 결합
한 라이브 강의 플랫폼을 개발하는 것을 목표로 한다. 또한, WebRTC 기반의 미디어 전
송 최적화와 클라우드 인프라를 도입하여 확장성을 개선하고, 동시 접속자가 많은 상
황에서도 안정적인 스트리밍 환경을 제공한다. 이러한 방식으로 학습자와 강사 간의
상호작용을 극대화하고, 플랫폼의 신뢰성과 사용자 경험을 향상시키고자 한다.
<br>궁극적으로, **본 프로젝트는 학습자들이 언제 어디서나 원활한 환경에서 교육에 집중
할 수 있도록 하여 더 나은 학습 경험을 제공하는 것을 지향한다.**

## ⚡ Try Refine

Start a new project with Refine in seconds using the following command:

```sh
npm create refine-app@latest my-refine-app
```

Or you can create a new project on your browser:

<a href="https://refine.dev/?playground=true" target="_blank">
  <img height="48" width="245" src="https://refine.ams3.cdn.digitaloceanspaces.com/assets/try-it-in-your-browser.png" />
</a>

## Quick Start

Here's Refine in action, the below code is an example of a simple CRUD application using Refine + React Router + Material UI:

```tsx
import React from "react";
import { Refine, useMany } from "@refinedev/core";
import { ThemedLayoutV2 } from "@refinedev/mui";
import dataProvider from "@refinedev/simple-rest";
import routerProvider from "@refinedev/react-router-v6";
import { BrowserRouter, Outlet, Route, Routes } from "react-router-dom";

import CssBaseline from "@mui/material/CssBaseline";

export default function App() {
  return (
    <BrowserRouter>
      <CssBaseline />
      <Refine
        dataProvider={dataProvider("https://api.fake-rest.refine.dev")}
        routerProvider={routerProvider}
        resources={[
          {
            name: "products",
            list: "/products",
          },
        ]}
      >
        <Routes>
          <Route
            element={
              <ThemedLayoutV2>
                <Outlet />
              </ThemedLayoutV2>
            }
          >
            <Route path="/products">
              <Route index element={<ProductList />} />
            </Route>
          </Route>
        </Routes>
      </Refine>
    </BrowserRouter>
  );
}

// src/pages/products/list.tsx

import { List, useDataGrid, DateField } from "@refinedev/mui";
import { DataGrid, GridColDef } from "@mui/x-data-grid";

export const ProductList = () => {
  const { dataGridProps } = useDataGrid();

  const { data: categories, isLoading } = useMany({
    resource: "categories",
    ids:
      dataGridProps?.rows?.map((item) => item?.category?.id).filter(Boolean) ??
      [],
    queryOptions: {
      enabled: !!dataGridProps?.rows,
    },
  });

  const columns = React.useMemo<GridColDef[]>(
    () => [
      { field: "id", headerName: "ID", type: "number" },
      { field: "name", flex: 1, headerName: "Name" },
      {
        field: "category",
        flex: 1,
        headerName: "Category",
        renderCell: ({ value }) =>
          isLoading
            ? "Loading..."
            : categories?.data?.find((item) => item.id === value?.id)?.title,
      },
      {
        field: "createdAt",
        flex: 1,
        headerName: "Created at",
        renderCell: ({ value }) => <DateField value={value} />,
      },
    ],
    [categories?.data, isLoading],
  );

  return (
    <List>
      <DataGrid {...dataGridProps} columns={columns} autoHeight />
    </List>
  );
};
```

The result will look like this:

[![Refine + Material UI Example](https://refine.ams3.cdn.digitaloceanspaces.com/assets/refine-mui-simple-example-screenshot-rounded.webp)](https://refine.new/preview/c85442a8-8df1-4101-a09a-47d3ca641798)

## Use cases

**Refine** shines on _data-intensive⚡_ enterprise B2B applications like **admin panels**, **dashboards** and **internal tools**. Thanks to the built-in **SSR support**, it can also power _customer-facing_ applications like **storefronts**.

You can take a look at some live examples that can be built using **Refine** from scratch:

- [Fully-functional CRM Application](https://refine.dev/templates/crm-application/)
- [Fully-functional Admin Panel](https://refine.dev/templates/react-admin-panel/)
- [Win95 Style Admin panel 🪟](https://refine.dev/templates/win-95-style-admin-panel/)
- [PDF Invoice Generator](https://refine.dev/templates/react-pdf-invoice-generator/)
- [Medium Clone - Real World Example](https://refine.dev/templates/react-crud-app/)
- [Multitenancy Example](https://refine.dev/templates/multitenancy-strapi/)
- [Storefront](https://refine.dev/templates/next-js-ecommerce-store/)
- [Refer to templates page for more examples](https://refine.dev/templates/)
- [More **Refine** powered different usage scenarios can be found here](https://refine.dev/docs/examples#other-examples)

## Key Features

- **강의 탐색 및 수강 신청**
사용자들은 플랫폼 메인 페이지에서 다양한 개설된 강의를 둘러볼 수 있다. 강의 소개
페이지에서는 강의 내용, 커리큘럼, 강사 정보 등을 자세히 확인할 수 있고, 관심 있는
강의를 발견하면 “수강 신청” 버튼을 통해 간편하게 등록할 수 있다.

-  **강의 대시보드**
강의에 등록한 수강생들은 강의 대시보드를 통해 강의 개요, 라이브 강의 일정, 공지사
항 등 모든 강의 정보를 한눈에 확인할 수 있다. 예정된 라이브 강의가 오픈되면 “라이
브 강의 입장" 버튼을 통해 바로 강의실에 입장할 수 있다.
강사와 수강생의 대시보드는 역할에 따라 구분되어 있다. 강사는 자신의 대시보드에서
강의 정보를 수정하거나 수강생을 관리할 수 있다. 수강생은 강의 일정을 확인하고 자
료를 열람할 수 있다.

- **라이브 강의 및 상호 작용**
라이브 강의 페이지에서는 실시간 영상 스트리밍을 통해 생동감 있는 강의를 제공한
다. 수강생들은 실시간 채팅 기능을 활용하여 강사와 소통하고 질문을 주고받을 수 있
다. 강사는 라이브 중에 프레젠테이션이나 문서 등을 공유하며 강의를 진행할 수 있다.

- **마이 페이지 및 개인 서비스**
사용자들은 마이 페이지에서 프로필 정보를 수정할 수 있다. 또한 현재 수강중인 강의
를 확인하고, 강의를 만들 수도 있다. 수강생은 자신이 등록한 강의를 확인하고 해당
강의에 쉽게 접근할 수 있다. 강사는 새로운 강의를 생성하고 관리할 수 있으며, 자신
이 만든 강의 목록을 열람하고 수정할 수 있다.

## Learning Refine

- Navigate to the [Tutorial](https://refine.dev/docs/tutorial/introduction/index/) on building comprehensive CRUD application guides you through each step of the process.
- Visit the [Guides & Concepts](https://refine.dev/docs/guides-concepts/general-concepts/) to get informed about the fundamental concepts.
- Read more on [Advanced Tutorials](https://refine.dev/docs/advanced-tutorials/) for different usage scenarios.

### Architecture

![architecture]({{site.url}}/PostImages/2024-10-22-OceanAcademy/architecture.png)


### 데모영상

<iframe width="560" height="315" src="https://www.youtube.com/embed/vAiGR7wuHDE?si=n7wtvJx-Y7rJ4D-4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Contributors ♥️ Thanks


<br/>

<a href="https://github.com/refinedev/refine/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=refinedev/refine&max=400&columns=20" />
</a>

## License

Licensed under the MIT License, Copyright © 2024-present Nemo