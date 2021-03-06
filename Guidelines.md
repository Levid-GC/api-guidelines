# Microsoft REST API 指南
## Microsoft REST API 指南工作组

|                              |                                        |                                          |
|:-----------------------------|:---------------------------------------|:-----------------------------------------|
| Dave Campbell (CTO C+E)      | Rick Rashid (CTO ASG)                  | John Shewchuk (Technical Fellow, TED HQ) |
| Mark Russinovich (CTO Azure) | Steve Lucco (Technical Fellow, DevDiv) | Murali Krishnaprasad (Azure App Plat)    |
| Rob Howard (ASG)             | Peter Torr  (OSG)                      | Chris Mullins (ASG)                      |

<div style="font-size:150%">
文档编辑者：John Gossman (C+E), Chris Mullins (ASG), Gareth Jones (ASG), Rob Dolin (C+E), Mark Stafford (C+E)<br/>
</div>



# Microsoft REST API 指南
## 1 摘要
Microsoft REST API 指南，是一种设计原则，用于鼓励应用开发者通过 RESTful HTTP 接口来访问资源。为了在遵循 Microsoft REST API 指南的平台上为开发者提供最流畅的体验，REST API **应该**遵循一致的设计指导原则，有利于其易用性和直观性。

Microsoft REST API **应该**遵循本文档建立的指南，以便保持 RESTful 接口开发的一致性。

## 2 目录
<!-- TOC depthFrom:1 depthTo:3 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Microsoft REST API 指南](#microsoft-rest-api-指南)
	- [Microsoft REST API 指南工作组](#microsoft-rest-api-指南工作组)
- [Microsoft REST API 指南](#microsoft-rest-api-指南-1)
	- [1 摘要](#1-摘要)
	- [2 目录](#2-目录)
	- [3 介绍](#3-介绍)
		- [3.1 推荐阅读](#31-推荐阅读)
	- [4    指南解释](#4-指南解释)
		- [4.1    应用指南](#41-应用指南)
		- [4.2    现有服务和服务版本控制指南](#42-现有服务和服务版本控制指南)
		- [4.3    要求性语言](#43-要求性语言)
		- [4.4    许可](#44-许可)
	- [5 分类](#5-分类)
		- [5.1    错误](#51-错误)
		- [5.2    故障](#52-故障)
		- [5.3    延迟](#53-延迟)
		- [5.4    完成时间](#54-完成时间)
		- [5.5    耗时 API 故障](#55-耗时-api-故障)
	- [6    客户端指南](#6-客户端指南)
		- [6.1    忽略规则](#61-忽略规则)
		- [6.2    变量顺序规则](#62-变量顺序规则)
		- [6.3    静默失败规则](#63-静默失败规则)
	- [7    Consistency fundamentals](#7-consistency-fundamentals)
		- [7.1    URL structure](#71-url-structure)
		- [7.2    URL length](#72-url-length)
		- [7.3    Canonical identifier](#73-canonical-identifier)
		- [7.4    Supported methods](#74-supported-methods)
		- [7.5    Standard request headers](#75-standard-request-headers)
		- [7.6    Standard response headers](#76-standard-response-headers)
		- [7.7    Custom headers](#77-custom-headers)
		- [7.8    Specifying headers as query parameters](#78-specifying-headers-as-query-parameters)
		- [7.9    PII parameters](#79-pii-parameters)
		- [7.10   Response formats](#710-response-formats)
		- [7.11   HTTP Status Codes](#711-http-status-codes)
		- [7.12   Client library optional](#712-client-library-optional)
	- [8    CORS](#8-cors)
		- [8.1    Client guidance](#81-client-guidance)
		- [8.2    Service guidance](#82-service-guidance)
	- [9    集合](#9-集合)
		- [9.1    元素 key](#91-元素-key)
		- [9.2    序列化](#92-序列化)
		- [9.3    集合 URL 模式](#93-集合-url-模式)
		- [9.4    大数据量集合](#94-大数据量集合)
		- [9.5    集合的更改](#95-集合的更改)
		- [9.6    集合的排序](#96-集合的排序)
		- [9.7    Filtering](#97-filtering)
		- [9.8    Pagination](#98-pagination)
		- [9.9    Compound collection operations](#99-compound-collection-operations)
	- [10    Delta queries](#10-delta-queries)
		- [10.1    Delta links](#101-delta-links)
		- [10.2    Entity representation](#102-entity-representation)
		- [10.3    Obtaining a delta link](#103-obtaining-a-delta-link)
		- [10.4    Contents of a delta link response](#104-contents-of-a-delta-link-response)
		- [10.5    Using a delta link](#105-using-a-delta-link)
	- [11    JSON standardizations](#11-json-standardizations)
		- [11.1    JSON formatting standardization for primitive types](#111-json-formatting-standardization-for-primitive-types)
		- [11.2    Guidelines for dates and times](#112-guidelines-for-dates-and-times)
		- [11.3    JSON serialization of dates and times](#113-json-serialization-of-dates-and-times)
		- [11.4    Durations](#114-durations)
		- [11.5    Intervals](#115-intervals)
		- [11.6    Repeating intervals](#116-repeating-intervals)
	- [12    Versioning](#12-versioning)
		- [12.1    Versioning formats](#121-versioning-formats)
		- [12.2    When to version](#122-when-to-version)
		- [12.3    Definition of a breaking change](#123-definition-of-a-breaking-change)
	- [13    Long running operations](#13-long-running-operations)
		- [13.1    Resource based long running operations (RELO)](#131-resource-based-long-running-operations-relo)
		- [13.2    Stepwise long running operations](#132-stepwise-long-running-operations)
		- [13.3    Retention policy for operation results](#133-retention-policy-for-operation-results)
	- [14    Push notifications via webhooks](#14-push-notifications-via-webhooks)
		- [14.1    Scope](#141-scope)
		- [14.2    Principles](#142-principles)
		- [14.3    Types of subscriptions](#143-types-of-subscriptions)
		- [14.4    Call sequences](#144-call-sequences)
		- [14.5    Verifying subscriptions](#145-verifying-subscriptions)
		- [14.6    Receiving notifications](#146-receiving-notifications)
		- [14.7    Managing subscriptions programmatically](#147-managing-subscriptions-programmatically)
		- [14.8    Security](#148-security)
	- [15    Unsupported requests](#15-unsupported-requests)
		- [15.1    Essential guidance](#151-essential-guidance)
		- [15.2    Feature allow list](#152-feature-allow-list)
	- [16    Naming guidelines](#16-naming-guidelines)
		- [16.1    Approach](#161-approach)
		- [16.2    Casing](#162-casing)
		- [16.3    Names to avoid](#163-names-to-avoid)
		- [16.4    Forming compound names](#164-forming-compound-names)
		- [16.5    Identity properties](#165-identity-properties)
		- [16.6    Date and time properties](#166-date-and-time-properties)
		- [16.7    Name properties](#167-name-properties)
		- [16.8    Collections and counts](#168-collections-and-counts)
		- [16.9    Common property names](#169-common-property-names)
	- [17     Appendix](#17-appendix)
		- [17.1    Sequence diagram notes](#171-sequence-diagram-notes)

<!-- /TOC -->

## 3 介绍
开发者通过 HTTP 接口来访问多数微软云平台资源。
虽然每个服务通常提供了语言相关的框架用于封装它们的 API ，但所有它们的操作本质上还是归结于 HTTP 请求。
微软必须支持更广泛的客户端并且服务不能依赖于每个开发环境中的富框架。
因此这个指南的一个目标就是确保 Microsoft REST API 能够简单且一致地被任何具有基本 HTTP 支持的客户端使用。

为了给开发者提供尽可能流畅的体验，那么这些 API 遵循一致的设计指南是很重要的，这会使得使用它们很简单且直观。本文档建立的指南需要 Microsoft REST API 开发者来遵循，以便能够一致地开发这样的 API 。

一致性的好处也是累积起来的，一致性可以允许团队利用公共代码、模式、文档和设计决策。

这些指导旨在实现以下目标：
- 为微软所有的 API 端点定义一致的体验和模式。
- 尽可能地严格遵守行业内 REST/HTTP 的最佳实践。*
- 让所有应用程序开发人员都可以通过 REST 接口访问微软的服务。
- 允许服务开发人员利用其他服务的前期工作来实现、测试和记录所定义的 REST 端点。
- 允许合作伙伴（例如，非微软实体）为自己的 REST 端点设计使用这些指导原则。

*注意：这些指导原则是为了与符合 REST 体系结构风格的构建服务相一致的，尽管它们没有处理或要求构建遵循 REST 约束的服务。
整个文档中使用的 "REST" 术语指的是那些以 REST 为指导思想而不是遵照书本的服务。*

### 3.1 推荐阅读
理解 REST 体系结构风格背后的哲学是为了开发良好的基于 HTTP 的服务。
如果你刚接触 RESTful 设计，那么这里有一些好的资源可以参考：

[REST on Wikipedia][rest-on-wikipedia] -- REST 的一般定义和核心思想的概述。

[REST Dissertation][fielding] -- Roy Fielding 在网络架构中关于 REST 章节的论文，"Architectural Styles and the Design of Network-based Software Architectures"

[RFC 7231][rfc-7231] -- 定义 HTTP/1.1 语义的规范，被认为是权威的资源。

[REST in Practice][rest-in-practice] -- 关于 REST 基础的书。

## 4 指南解释
### 4.1 应用指南
这些指南适用于微软或任何合作伙伴公开的 REST API 。
私有或内部的 API 也应该遵循这些指南，因为内部服务最终也趋向于公开。
一致性不仅对外部客户和内部服务消费者都有价值，而且这些指南提供了对任何服务都有用的最佳实践。

我们对这些指南的豁免有合法的理由。
显然，实现或必须与外部定义的 REST API 进行互操作的 REST 服务必须与该 API 兼容，而不一定是这些指南。
一些服务**可能**还具有特殊的性能需求，需要不同的格式，比如二进制协议。

### 4.2 现有服务和服务版本控制指南
仅为了满足这些原则而对制定这些指南之前的服务进行修改，我们是不推荐的。
当兼容性会被破坏时，该服务**应该**尝试在发行下一个版本时兼容。
当一个服务添加一个新的 API 时，这个 API 应该与同一版本的其它 API 保持一致。
因此，如果结合该指南的 1.0 版本编写了一个服务，那么新添加的 API 也**应该**遵循 1.0 版本。然后，该服务就可以结合最新版本的指南并升级到服务的下一个主要版本。

### 4.3 要求性语言
本文档中的 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", 和 "OPTIONAL" 关键词解释参见 [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) 。

### 4.4 许可

本项工作的许可采用 Creative Commons Attribution 4.0 International License 。
想要查看此许可，参见 http://creativecommons.org/licenses/by/4.0/ 或发送邮件至 Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

## 5 分类
作为 Microsoft REST API 指南的一部分，服务**必须**遵循以下的分类定义。

### 5.1 错误
错误，或者更准确地来说是服务错误，指的就是客户端向服务传递非法的数据但是服务能够*正确地*拒绝这个非法数据。
比如，非法的凭据，不正确的参数，未知的版本号，等等类似的错误。
通常客户端如果传递不正确或者非法数据时，则使用 "4xx" HTTP 错误码作为响应。

错误并*不会*提升 API 的整体可用性。

### 5.2 故障
故障，或者更准确地来说是服务故障，指的就是服务没能够对合法的客户端请求做出正确的响应。
通常的结果就是 "5xx" HTTP 错误码。

故障*能够*提升 API 的整体可用性。

由于速率限制或者配额分配失败导致的失败**禁止**被认为是故障。
因服务请求的快速失败而导致的失败（通常是为了自身的保护）可以认为是故障。

### 5.3 延迟
延迟指的就是一个特定的 API 调用需要花费多长时间来完成，尽可能在客户端进行测量它。
同步和异步的 API 使用同样的度量标准。
对于耗时的调用，这个延迟是从初始的请求开始测算，直到调用任务（并不是整个操作）完成为止。

### 5.4 完成时间
提供耗时操作的服务**必须**对这些操作使用“完成时间”的度量标准进行跟踪。

### 5.5 耗时 API 故障
对于一个耗时的 API ，极有可能在初始化请求和获取结果的请求时都收到 200 状态码，但是底层的操作可能已经失败了。
耗时的故障**必须**作为故障纳入整体可用性度量指标。

## 6 客户端指南
为了确保客户端能够使用具有最佳体验的 REST 服务，客户端**应该**遵循以下最佳实践：

### 6.1 忽略规则
对于低耦合的客户端来说，在发送请求前，数据的具体形态是未知的，如果服务器返回了一些不是客户端所期望的内容，客户端**必须**安全地忽略它们。

一些服务**可能**在未更改版本号的情况下在响应结果中添加了其它字段。如果服务确实这么做了，那么这**必须**在文档中明确指出，客户端也**必须**忽略未知的字段。

### 6.2 变量顺序规则
客户端**禁止**依赖 JSON 服务响应中的数据顺序。
举个例子，客户端**应该**能适应一个 JSON 对象内的字段重新排序。
如果在服务支持的条件下，客户端**可能**请求以指定顺序返回数据。
举个例子，服务**可能**支持 *$orderBy* 查询字符串参数的使用，它用于指定一个 JSON 数组中元素的顺序。
服务也**可能**在服务契约中显式指定了一些元素的排序方式。
举个例子，一个服务**可能**总是将返回的 JSON 对象的 "type" 信息作为第一个字段，用于简化客户端对响应的解析。
客户端**可能**对于服务明确标识的排序行为有依赖。

### 6.3 静默失败规则
客户端请求**可选**的服务器端功能（比如可选的 HTTP 报头）**必须**对它保持一定的弹性，而不要忽略了特定的功能。 

## 7 Consistency fundamentals
### 7.1 URL structure
Humans SHOULD be able to easily read and construct URLs.

This facilitates discovery and eases adoption on platforms without a well-supported client library.

An example of a well-structured URL is:

```
https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox
```

An example URL that is not friendly is:

```
https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```

A frequent pattern that comes up is the use of URLs as values.
Services MAY use URLs as values.
For example, the following is acceptable:

```
https://api.contoso.com/v1.0/items?url=https://resources.contoso.com/shoes/fancy
```

### 7.2 URL length
The HTTP 1.1 message format, defined in RFC 7230, in section [3.1.1][rfc-7230-3-1-1], defines no length limit on the Request Line, which includes the target URL.
From the RFC:

> HTTP does not place a predefined limit on the length of a
   request-line. [...] A server that receives a request-target longer than any URI it wishes to parse MUST respond
   with a 414 (URI Too Long) status code.

Services that can generate URLs longer than 2,083 characters MUST make accommodations for the clients they wish to support.
Here are some sources for determining what target clients support:

 * [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)
 * [https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/)
 
Also note that some technology stacks have hard and adjustable url limits, so keep this in mind as you design your services.

### 7.3 Canonical identifier
In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier.
It MAY be necessary to interact with the service to obtain a stable URL from the friendly name for the resource, as in the case of the "/my" shortcut used by some services.

The stable identifier is not required to be a GUID.

An example of a URL containing a canonical identifier is:

```
https://api.contoso.com/v1.0/people/7011042402/inbox
```

### 7.4 Supported methods
Operations MUST use the proper HTTP methods whenever possible, and operation idempotency MUST be respected.
HTTP methods are frequently referred to as the HTTP verbs.
The terms are synonymous in this context, however the HTTP specification uses the term method.

Below is a list of methods that Microsoft REST services SHOULD support.
Not all resources will support all methods, but all resources using the methods below MUST conform to their usage.

Method  | Description                                                                                                                | Is Idempotent
------- | -------------------------------------------------------------------------------------------------------------------------- | -------------
GET     | Return the current value of an object                                                                                      | True
PUT     | Replace an object, or create a named object, when applicable                                                               | True
DELETE  | Delete an object                                                                                                           | True
POST    | Create a new object based on the data provided, or submit a command                                                        | False
HEAD    | Return metadata of an object for a GET response. Resources that support the GET method MAY support the HEAD method as well | True
PATCH   | Apply a partial update to an object                                                                                        | False
OPTIONS | Get information about a request; see below for details.                                                                    | True

<small>Table 1</small>

#### 7.4.1 POST
POST operations SHOULD support the Location response header to specify the location of any created resource that was not explicitly named, via the Location header.

As an example, imagine a service that allows creation of hosted servers, which will be named by the service:

```http
POST http://api.contoso.com/account1/servers
```

The response would be something like:

```http
201 Created
Location: http://api.contoso.com/account1/servers/server321
```

Where "server321" is the service-allocated server name.

Services MAY also return the full metadata for the created item in the response.

#### 7.4.2 PATCH
PATCH has been standardized by IETF as the method to be used for updating an existing object incrementally (see [RFC 5789][rfc-5789]).
Microsoft REST API Guidelines compliant APIs SHOULD support PATCH.

#### 7.4.3 Creating resources via PATCH (UPSERT semantics)
Services that allow callers to specify key values on create SHOULD support UPSERT semantics, and those that do MUST support creating resources using PATCH.
Because PUT is defined as a complete replacement of the content, it is dangerous for clients to use PUT to modify data.
Clients that do not understand (and hence ignore) properties on a resource are not likely to provide them on a PUT when trying to update a resource, hence such properties could be inadvertently removed.
Services MAY optionally support PUT to update existing resources, but if they do they MUST use replacement semantics (that is, after the PUT, the resource's properties MUST match what was provided in the request, including deleting any server properties that were not provided).

Under UPSERT semantics, a PATCH call to a nonexistent resource is handled by the server as a "create," and a PATCH call to an existing resource is handled as an "update." To ensure that an update request is not treated as a create or vice-versa, the client MAY specify precondition HTTP headers in the request.
The service MUST NOT treat a PATCH request as an insert if it contains an If-Match header and MUST NOT treat a PATCH request as an update if it contains an If-None-Match header with a value of "*".

If a service does not support UPSERT, then a PATCH call against a resource that does not exist MUST result in an HTTP "409 Conflict" error.

#### 7.4.4 Options and link headers
OPTIONS allows a client to retrieve information about a resource, at a minimum by returning the Allow header denoting the valid methods for this resource.

In addition, services SHOULD include a Link header (see [RFC 5988][rfc-5988]) to point to documentation for the resource in question:

```http
Link: <{help}>; rel="help"
```

Where {help} is the URL to a documentation resource.

For examples on use of OPTIONS, see [preflighting CORS cross-domain calls][cors-preflight].

### 7.5 Standard request headers
The table of request headers below SHOULD be used by Microsoft REST API Guidelines services.
Using these headers is not mandated, but if used they MUST be used consistently.

All header values MUST follow the syntax rules set forth in the specification where the header field is defined.
Many HTTP headers are defined in [RFC7231][rfc-7231], however a complete list of approved headers can be found in the [IANA Header Registry][IANA-headers]."

Header                            | Type                                  | Description
--------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Authorization                     | String                                           | Authorization header for the request
Date                              | Date                                             | Timestamp of the request, based on the client's clock, in [RFC 5322][rfc-5322-3-3] date and time format.  The server SHOULD NOT make any assumptions about the accuracy of the client's clock.  This header MAY be included in the request, but MUST be in this format when supplied.  Greenwich Mean Time (GMT) MUST be used as the time zone reference for this header when it is provided.  For example: `Wed, 24 Aug 2016 18:41:30 GMT`.  Note that GMT is exactly equal to UTC (Coordinated Universal Time) for this purpose.
Accept                            | Content type                                     | The requested content type for the response such as: <ul><li>application/xml</li><li>text/xml</li><li>application/json</li><li>text/javascript (for JSONP)</li></ul>Per the HTTP guidelines, this is just a hint and responses MAY have a different content type, such as a blob fetch where a successful response will just be the blob stream as the payload. For services following OData, the preference order specified in OData SHOULD be followed.
Accept-Encoding                   | Gzip, deflate                                    | REST endpoints SHOULD support GZIP and DEFLATE encoding, when applicable. For very large resources, services MAY ignore and return uncompressed data.
Accept-Language                   | "en", "es", etc.                                 | Specifies the preferred language for the response. Services are not required to support this, but if a service supports localization it MUST do so through the Accept-Language header.
Accept-Charset                    | Charset type like "UTF-8"                        | Default is UTF-8, but services SHOULD be able to handle ISO-8859-1.
Content-Type                      | Content type                                     | Mime type of request body (PUT/POST/PATCH)
Prefer                            | return=minimal, return=representation            | If the return=minimal preference is specified, services SHOULD return an empty body in response to a successful insert or update. If return=representation is specified, services SHOULD return the created or updated resource in the response. Services SHOULD support this header if they have scenarios where clients would sometimes benefit from responses, but sometimes the response would impose too much of a hit on bandwidth.
If-Match, If-None-Match, If-Range | String                                           | Services that support updates to resources using optimistic concurrency control MUST support the If-Match header to do so. Services MAY also use other headers related to ETags as long as they follow the HTTP specification.

### 7.6 Standard response headers
Services SHOULD return the following response headers, except where noted in the "required" column.

Response Header    | Required                                      | Description
------------------ | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Date               | All responses                                 | Timestamp the response was processed, based on the server's clock, in [RFC 5322][rfc-5322-3-3] date and time format.  This header MUST be included in the response.  Greenwich Mean Time (GMT) MUST be used as the time zone reference for this header.  For example: `Wed, 24 Aug 2016 18:41:30 GMT`. Note that GMT is exactly equal to UTC (Coordinated Universal Time) for this purpose.
Content-Type       | All responses                                 | The content type
Content-Encoding   | All responses                                 | GZIP or DEFLATE, as appropriate
Preference-Applied | When specified in request                     | Whether a preference indicated in the Prefer request header was applied
ETag               | When the requested resource has an entity tag | The ETag response-header field provides the current value of the entity tag for the requested variant. Used with If-Match, If-None-Match and If-Range to implement optimistic concurrency control.

### 7.7 Custom headers
Custom headers MUST NOT be required for the basic operation of a given API.

Some of the guidelines in this document prescribe the use of nonstandard HTTP headers.
In addition, some services MAY need to add extra functionality, which is exposed via HTTP headers.
The following guidelines help maintain consistency across usage of custom headers.

Headers that are not standard HTTP headers MUST have one of two formats:

1. A generic format for headers that are registered as "provisional" with IANA ([RFC 3864][rfc-3864])
2. A scoped format for headers that are too usage-specific for registration

These two formats are described below.

### 7.8 Specifying headers as query parameters
Some headers pose challenges for some scenarios such as AJAX clients, especially when making cross-domain calls where adding headers MAY not be supported.
As such, some headers MAY be accepted as Query Parameters in addition to headers, with the same naming as the header:

Not all headers make sense as query parameters, including most standard HTTP headers.

The criteria for considering when to accept headers as parameters are:

1. Any custom headers MUST be also accepted as parameters.
2. Required standard headers MAY be accepted as parameters.
3. Required headers with security sensitivity (e.g., Authorization header) MIGHT NOT be appropriate as parameters; the service owner SHOULD evaluate these on a case-by-case basis.

The one exception to this rule is the Accept header.
It's common practice to use a scheme with simple names instead of the full functionality described in the HTTP specification for Accept.

### 7.9 PII parameters
Consistent with their organization's privacy policy, clients SHOULD NOT transmit personally identifiable information (PII) parameters in the URL (as part of path or query string) because this information can be inadvertently exposed via client, network, and server logs and other mechanisms.

Consequently, a service SHOULD accept PII parameters transmitted as headers.

However, there are many scenarios where the above recommendations cannot be followed due to client or software limitations.
To address these limitations, services SHOULD also accept these PII parameters as part of the URL consistent with the rest of these guidelines.

Services that accept PII parameters -- whether in the URL or as headers -- SHOULD be compliant with privacy policy specified by their organization's engineering leadership.
This will typically include recommending that clients prefer headers for transmission and implementations adhere to special precautions to ensure that logs and other service data collection are properly handled.

### 7.10 Response formats
For organizations to have a successful platform, they must serve data in formats developers are accustomed to using, and in consistent ways that allow developers to handle responses with common code.

Web-based communication, especially when a mobile or other low-bandwidth client is involved, has moved quickly in the direction of JSON for a variety of reasons, including its tendency to be lighter weight and its ease of consumption with JavaScript-based clients.

JSON property names SHOULD be camelCased.

Services SHOULD provide JSON as the default encoding.

#### 7.10.1 Clients-specified response format
In HTTP, response format SHOULD be requested by the client using the Accept header.
This is a hint, and the server MAY ignore it if it chooses to, even if this isn't typical of well-behaved servers.
Clients MAY send multiple Accept headers and the service MAY choose one of them.

The default response format (no Accept header provided) SHOULD be application/json, and all services MUST support application/json.

Accept Header    | Response type                      | Notes
---------------- | ---------------------------------- | -------------------------------------------
application/json | Payload SHOULD be returned as JSON | Also accept text/javascript for JSONP cases

```http
GET https://api.contoso.com/v1.0/products/user
Accept: application/json
```

#### 7.10.2 Error condition responses
For nonsuccess conditions, developers SHOULD be able to write one piece of code that handles errors consistently across different Microsoft REST API Guidelines services.
This allows building of simple and reliable infrastructure to handle exceptions as a separate flow from successful responses.
The following is based on the OData v4 JSON spec.
However, it is very generic and does not require specific OData constructs.
APIs SHOULD use this format even if they are not using other OData constructs.

The error response MUST be a single JSON object.
This object MUST have a name/value pair named "error." The value MUST be a JSON object.

This object MUST contain name/value pairs with the names "code" and "message," and it MAY contain name/value pairs with the names "target," "details" and "innererror."

The value for the "code" name/value pair is a language-independent string.
Its value is a service-defined error code that SHOULD be human-readable.
This code serves as a more specific indicator of the error than the HTTP error code specified in the response.
Services SHOULD have a relatively small number (about 20) of possible values for "code," and all clients MUST be capable of handling all of them.
Most services will require a much larger number of more specific error codes, which are not interesting to all clients.
These error codes SHOULD be exposed in the "innererror" name/value pair as described below.
Introducing a new value for "code" that is visible to existing clients is a breaking change and requires a version increase.
Services can avoid breaking changes by adding new error codes to "innererror" instead.

The value for the "message" name/value pair MUST be a human-readable representation of the error.
It is intended as an aid to developers and is not suitable for exposure to end users.
Services wanting to expose a suitable message for end users MUST do so through an [annotation][odata-json-annotations] or custom property.
Services SHOULD NOT localize "message" for the end user, because doing so MAY make the value unreadable to the app developer who may be logging the value, as well as make the value less searchable on the Internet.

The value for the "target" name/value pair is the target of the particular error (e.g., the name of the property in error).

The value for the "details" name/value pair MUST be an array of JSON objects that MUST contain name/value pairs for "code" and "message," and MAY contain a name/value pair for "target," as described above.
The objects in the "details" array usually represent distinct, related errors that occurred during the request.
See example below.

The value for the "innererror" name/value pair MUST be an object.
The contents of this object are service-defined.
Services wanting to return more specific errors than the root-level code MUST do so by including a name/value pair for "code" and a nested "innererror." Each nested "innererror" object represents a higher level of detail than its parent.
When evaluating errors, clients MUST traverse through all of the nested "innererrors" and choose the deepest one that they understand.
This scheme allows services to introduce new error codes anywhere in the hierarchy without breaking backwards compatibility, so long as old error codes still appear.
The service MAY return different levels of depth and detail to different callers.
For example, in development environments, the deepest "innererror" MAY contain internal information that can help debug the service.
To guard against potential security concerns around information disclosure, services SHOULD take care not to expose too much detail unintentionally.
Error objects MAY also include custom server-defined name/value pairs that MAY be specific to the code.
Error types with custom server-defined properties SHOULD be declared in the service's metadata document.
See example below.

Error responses MAY contain [annotations][odata-json-annotations] in any of their JSON objects.

We recommend that for any transient errors that may be retried, services SHOULD include a Retry-After HTTP header indicating the minimum number of seconds that clients SHOULD wait before attempting the operation again.

##### ErrorResponse : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`error` | Error | ✔ | The error object.

##### Error : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String (enumerated) | ✔ | One of a server-defined set of error codes.
`message` | String | ✔ | A human-readable representation of the error.
`target` | String |  | The target of the error.
`details` | Error[] |  | An array of details about specific errors that led to this reported error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### InnerError : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String |  | A more specific error code than was provided by the containing error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### Examples

Example of "innererror":

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Previous passwords may not be reused",
    "target": "password",
    "innererror": {
      "code": "PasswordError",
      "innererror": {
        "code": "PasswordDoesNotMeetPolicy",
        "minLength": "6",
        "maxLength": "64",
        "characterTypes": ["lowerCase","upperCase","number","symbol"],
        "minDistinctCharacterTypes": "2",
        "innererror": {
          "code": "PasswordReuseNotAllowed"
        }
      }
    }
  }
}
```

In this example, the most basic error code is "BadArgument," but for clients that are interested, there are more specific error codes in "innererror."
The "PasswordReuseNotAllowed" code may have been added by the service at a later date, having previously only returned "PasswordDoesNotMeetPolicy."
Existing clients do not break when the new error code is added, but new clients MAY take advantage of it.
The "PasswordDoesNotMeetPolicy" error also includes additional name/value pairs that allow the client to determine the server's configuration, validate the user's input programmatically, or present the server's constraints to the user within the client's own localized messaging.

Example of "details":

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Multiple errors in ContactInfo data",
    "target": "ContactInfo",
    "details": [
      {
        "code": "NullValue",
        "target": "PhoneNumber",
        "message": "Phone number must not be null"
      },
      {
        "code": "NullValue",
        "target": "LastName",
        "message": "Last name must not be null"
      },
      {
        "code": "MalformedValue",
        "target": "Address",
        "message": "Address is not valid"
      }
    ]
  }
}
```

In this example there were multiple problems with the request, with each individual error listed in "details."

### 7.11 HTTP Status Codes
Standard HTTP Status Codes SHOULD be used; see the HTTP Status Code definitions for more information.

### 7.12 Client library optional
Developers MUST be able to develop on a wide variety of platforms and languages, such as Windows, macOS, Linux, C#, Python, Node.js, and Ruby.

Services SHOULD be able to be accessed from simple HTTP tools such as curl without significant effort.

Service developer portals SHOULD provide the equivalent of "Get Developer Token" to facilitate experimentation and curl support.

## 8 CORS
Services compliant with the Microsoft REST API Guidelines MUST support [CORS (Cross Origin Resource Sharing)][cors].
Services SHOULD support an allowed origin of CORS * and enforce authorization through valid OAuth tokens.
Services SHOULD NOT support user credentials with origin validation.
There MAY be exceptions for special cases.

### 8.1 Client guidance
Web developers usually don't need to do anything special to take advantage of CORS.
All of the handshake steps happen invisibly as part of the standard XMLHttpRequest calls they make.

Many other platforms, such as .NET, have integrated support for CORS.

#### 8.1.1 Avoiding preflight
Because the CORS protocol can trigger preflight requests that add additional round trips to the server, performance-critical apps might be interested in avoiding them.
The spirit behind CORS is to avoid preflight for any simple cross-domain requests that old non-CORS-capable browsers were able to make.
All other requests require preflight.

A request is "simple" and avoids preflight if its method is GET, HEAD or POST, and if it doesn't contain any request headers besides Accept, Accept-Language and Content-Language.
For POST requests, the Content-Type header is also allowed, but only if its value is "application/x-www-form-urlencoded," "multipart/form-data" or "text/plain."
For any other headers or values, a preflight request will happen.

### 8.2 Service guidance
 At minimum, services MUST:
- Understand the Origin request header that browsers send on cross-domain requests, and the Access-Control-Request-Method request header that they send on preflight OPTIONS requests that check for access.
- If the Origin header is present in a request:
  - If the request uses the OPTIONS method and contains the Access-Control-Request-Method header, then it is a preflight request intended to probe for access before the actual request. Otherwise, it is an actual request. For preflight requests, beyond performing the steps below to add headers, services MUST perform no additional processing and MUST return a 200 OK. For non-preflight requests, the headers below are added in addition to the request's regular processing.
  - Add an Access-Control-Allow-Origin header to the response, containing the same value as the Origin request header. Note that this requires services to dynamically generate the header value. Resources that do not require cookies or any other form of [user credentials][cors-user-credentials] MAY respond with a wildcard asterisk (*) instead. Note that the wildcard is acceptable here only, and not for any of the other headers described below.
  - If the caller requires access to a response header that is not in the set of [simple response headers][cors-simple-headers] (Cache-Control, Content-Language, Content-Type, Expires, Last-Modified, Pragma), then add an Access-Control-Expose-Headers header containing the list of additional response header names the client should have access to.
  - If the request requires cookies, then add an Access-Control-Allow-Credentials header set to "true."
  - If the request was a preflight request (see first bullet), then the service MUST:
    - Add an Access-Control-Allow-Headers response header containing the list of request header names the client is permitted to use. This list need only contain headers that are not in the set of [simple request headers][cors-simple-headers] (Accept, Accept-Language, Content-Language). If there are no restrictions on headers the service accepts, the service MAY simply return the same value as the Access-Control-Request-Headers header sent by the client.
    - Add an Access-Control-Allow-Methods response header containing the list of HTTP methods the caller is permitted to use.

Add an Access-Control-Max-Age pref response header containing the number of seconds for which this preflight response is valid (and hence can be avoided before subsequent actual requests). Note that while it is customary to use a large value like 2592000 (30 days), many browsers self-impose a much lower limit (e.g., five minutes).

Because browser preflight response caches are notoriously weak, the additional round trip from a preflight response hurts performance.
Services used by interactive Web clients where performance is critical SHOULD avoid patterns that cause a preflight request
- For GET and HEAD calls, avoid requiring request headers that are not part of the simple set above. Allow them to be provided as query parameters instead.
  - The Authorization header is not part of the simple set, so the authentication token MUST be sent through the "access_token" query parameter instead, for resources requiring authentication. Note that passing authentication tokens in the URL is not recommended, because it can lead to the token getting recorded in server logs and exposed to anyone with access to those logs. Services that accept authentication tokens through the URL MUST take steps to mitigate the security risks, such as using short-lived authentication tokens, suppressing the auth token from getting logged, and controlling access to server logs.

- Avoid requiring cookies. XmlHttpRequest will only send cookies on cross-domain requests if the "withCredentials" attribute is set; this also causes a preflight request.
  - Services that require cookie-based authentication MUST use a "dynamic canary" to secure all APIs that accept cookies.

- For POST calls, prefer simple Content-Types in the set of ("application/x-www-form-urlencoded," "multipart/form-data," "text/plain") where applicable. Any other Content-Type will induce a preflight request.
  - Services MUST NOT contravene other API recommendations in the name of avoiding CORS preflight requests. In particular, in accordance with recommendations, most POST requests will actually require a preflight request due to the Content-Type.
  - If eliminating preflight is critical, then a service MAY support alternative mechanisms for data transfer, but the RECOMMENDED approach MUST also be supported.

In addition, when appropriate services MAY support the JSONP pattern for simple, GET-only cross-domain access.
In JSONP, services take a parameter indicating the format (_$format=json_) and a parameter indicating a callback (_$callback=someFunc_), and return a text/javascript document containing the JSON response wrapped in a function call with the indicated name.
More on JSONP at Wikipedia: [JSONP](https://en.wikipedia.org/wiki/JSONP).

## 9 集合
### 9.1 元素 key
服务**可能**对集合中每个元素的持久化标识符提供支持，在 JSON 中这种标识符**应该**使用 *id* 表示。这些持久化标识符通常用作元素的 key 。

支持持久化标识符的集合**可能**支持 delta 查询。

### 9.2 序列化
在 JSON 中集合以标准的数组形式表示。

### 9.3 集合 URL 模式
当集合处于顶级的时候那么它可以直接定位到服务根，而当集合处于其他资源的作用域下则作为其他资源的片段来定位。

示例：

```http
GET https://api.contoso.com/v1.0/people
```

无论如何，服务都**必须**支持 "/" 模式。

示例：

```http
GET https://{serviceRoot}/{collection}/{id}
```

其中：
*  {serviceRoot} - 主机（网站地址） + 服务根路径的组合
*  {collection} - 集合名，未经缩写的复数形式名称
*  {id} - 唯一的 id 属性值。使用 "/" 模式时，id **必须**是原始的不带有引号的 string/number/guid 值，但是必须经过正确地转译以融为 URL 的一部分。

#### 9.3.1    嵌套集合及其属性
集合元素**可能**会包含其他集合。
比如，一个用户集合可能会包含多个地址信息：

```http
GET https://api.contoso.com/v1.0/people/123/addresses
```

```json
{
  "value": [
    { "street": "1st Avenue", "city": "Seattle" },
    { "street": "124th Ave NE", "city": "Redmond" }
  ]
}
```

### 9.4 大数据量集合
随着数据量的增长，集合也越来越大。
为所有服务规划分页功能是非常重要的。
因此当集合具有多个分页信息时，序列化的负载**必须**包含一个合适且不透明的下一页 URL 。
更多信息参见分页指南。

对于任何给定的请求，客户端**必须**能够灵活应对分页的或未分页的集合数据。

```json
{
  "value":[
    { "id": "Item 1","price": 99.95,"sizes": null},
    { … },
    { … },
    { "id": "Item 99","price": 59.99,"sizes": null}
  ],
  "@nextLink": "{opaqueUrl}"
}
```

### 9.5 集合的更改
POST 请求不具有幂等性。
这就意味着两个具有严格相同负载的 POST 请求如果向同一个集合资源发送请求**可能**会导致多个元素的创建。
这种情况通常发生在元素 id 由服务器端生成的插入操作中。

举个例子，有如下请求：

```http
POST https://api.contoso.com/v1.0/people
```

该请求将会获得一个指示新创建的集合元素位置的响应：

```http
201 Created
Location: https://api.contoso.com/v1.0/people/123
```

再执行一次，很可能会导致另一个元素的创建：

```http
201 Created
Location: https://api.contoso.com/v1.0/people/124
```

然而，PUT 请求需要使用到集合元素对应的 key 来表示该元素：

```http
PUT https://api.contoso.com/v1.0/people/123
```

### 9.6 集合的排序
集合的查询**可能**会基于某个属性值进行排序。
该属性由 *$orderBy* 查询参数值决定。

*$orderBy* 参数值是以逗号分隔的表达式列表。
这种表达式的一种特殊情况就是一个属性路径会终止于一个基元属性。

表达式**可能**包含 "asc" 或 "desc" 后缀分别用于表示升序和降序，后缀与属性名以一个或多个空格分隔。 
如果没有指定 "asc" 或 "desc" ，服务**必须**对指定的属性执行升序排列。

NULL 值**必须**排在非 NULL 值前面。

元素**必须**先根据第一个表达式的结果值进行排序，然后再根据第二个表达式进行排序，以此类推。
排序顺序是属性类型的固有顺序。

示例：

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name
```

该请求返回所有按照 name 属性升序排列的 people 。

示例：

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc
```

该请求返回所有按照 name 属性降序排列的 people 。

子排序可以通过以逗号分隔的带有**可选**方向限定符的属性名列表来指定。

示例：

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc,hireDate
```

该请求返回所有按照 name 降序排列，再按照 hireDate 升序二次排列的 people 。

排序**必须**与过滤组合使用，比如：

```http
GET https://api.contoso.com/v1.0/people?$filter=name eq 'david'&$orderBy=hireDate
```

该请求返回所有姓名为 David ，并按照 hireDate 升序排列的 people 。

#### 9.6.1 排序表达式注解
各页面中的排序参数**必须**保持一致，客户端与服务器端分页都要与排序完全兼容。

如果服务不支持 *$orderBy* 表达式中的属性, 服务**必须**返回一个在 “响应未支持的请求” 部分定义好的错误消息。

### 9.7 Filtering
The _$filter_ querystring parameter allows clients to filter a collection of resources that are addressed by a request URL.
The expression specified with _$filter_ is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response.
Resources for which the expression evaluates to false or to null, or which reference properties that are unavailable due to permissions, are omitted from the response.

Example: return all Products whose Price is less than $10.00

```http
GET https://api.contoso.com/v1.0/products?$filter=price lt 10.00
```

The value of the _$filter_ option is a Boolean expression.

#### 9.7.1 Filter operations
Services that support _$filter_ SHOULD support the following minimal set of operations.

Operator             | Description           | Example
-------------------- | --------------------- | -----------------------------------------------------
Comparison Operators |                       |
eq                   | Equal                 | city eq 'Redmond'
ne                   | Not equal             | city ne 'London'
gt                   | Greater than          | price gt 20
ge                   | Greater than or equal | price ge 10
lt                   | Less than             | price lt 20
le                   | Less than or equal    | price le 100
Logical Operators    |                       |
and                  | Logical and           | price le 200 and price gt 3.5
or                   | Logical or            | price le 3.5 or price gt 200
not                  | Logical negation      | not price le 3.5
Grouping Operators   |                       |
( )                  | Precedence grouping   | (priority eq 1 or city eq 'Redmond') and price gt 100

#### 9.7.2 Operator examples
The following examples illustrate the use and semantics of each of the logical operators.

Example: all products with a name equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk'
```

Example: all products with a name not equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name ne 'Milk'
```

Example: all products with the name 'Milk' that also have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' and price lt 2.55
```

Example: all products that either have the name 'Milk' or have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' or price lt 2.55
```

Example 54: all products that have the name 'Milk' or 'Eggs' and have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=(name eq 'Milk' or name eq 'Eggs') and price lt 2.55
```

#### 9.7.3 Operator precedence
Services MUST use the following operator precedence for supported operators when evaluating _$filter_ expressions.
Operators are listed by category in order of precedence from highest to lowest.
Operators in the same category have equal precedence:

| Group           | Operator | Description           |
|:----------------|:---------|:----------------------|
| Grouping        | ( )      | Precedence grouping   |
| Unary           | not      | Logical Negation      |
| Relational      | gt       | Greater Than          |
|                 | ge       | Greater than or Equal |
|                 | lt       | Less Than             |
|                 | le       | Less than or Equal    |
| Equality        | eq       | Equal                 |
|                 | ne       | Not Equal             |
| Conditional AND | and      | Logical And           |
| Conditional OR  | or       | Logical Or            |

### 9.8 Pagination
RESTful APIs that return collections MAY return partial sets.
Consumers of these services MUST expect partial result sets and correctly page through to retrieve an entire set.

There are two forms of pagination that MAY be supported by RESTful APIs.
Server-driven paging mitigates against denial-of-service attacks by forcibly paginating a request over multiple response payloads.
Client-driven paging enables clients to request only the number of resources that it can use at a given time.

Sorting and Filtering parameters MUST be consistent across pages, because both client- and server-side paging is fully compatible with both filtering and sorting.

#### 9.8.1 Server-driven paging
Paginated responses MUST indicate a partial result by including a continuation token in the response.
The absence of a continuation token means that no additional pages are available.

Clients MUST treat the continuation URL as opaque, which means that query options may not be changed while iterating over a set of partial results.

Example:

```http
GET http://api.contoso.com/v1.0/people HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...],
  "@nextLink": "{opaqueUrl}"
}
```

#### 9.8.2 Client-driven paging
Clients MAY use _$top_ and _$skip_ query parameters to specify a number of results to return and an offset.
The server SHOULD honor the values specified by the client; however, clients MUST be prepared to handle responses that contain a different page size or contain a continuation token.

Note: If the server can't honor _$top_ and/or _$skip_, the server MUST return an error to the client informing about it instead of just ignoring the query options.
This will avoid the risk of the client making assumptions about the data returned.

Example:

```http
GET http://api.contoso.com/v1.0/people?$top=5&$skip=2 HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...]
}
```

#### 9.8.3 Additional considerations
**Stable order prerequisite:** Both forms of paging depend on the collection of items having a stable order.
The server MUST supplement any specified order criteria with additional sorts (typically by key) to ensure that items are always ordered consistently.

**Missing/repeated results:** Even if the server enforces a consistent sort order, results MAY be missing or repeated based on creation or deletion of other resources.
Clients MUST be prepared to deal with these discrepancies.
The server SHOULD always encode the record ID of the last read record, helping the client in the process of managing repeated/missing results.

**Combining client- and server-driven paging:** Note that client-driven paging does not preclude server-driven paging.
If the page size requested by the client is larger than the default page size supported by the server, the expected response would be the number of results specified by the client, paginated as specified by the server paging settings.

**Page Size:** Clients MAY request server-driven paging with a specific page size by specifying a _$maxpagesize_ preference.
The server SHOULD honor this preference if the specified page size is smaller than the server's default page size.

**Paginating embedded collections:** It is possible for both client-driven paging and server-driven paging to be applied to embedded collections.
If a server paginates an embedded collection, it MUST include additional continuation tokens as appropriate.

**Recordset count:** Developers who want to know the full number of records across all pages, MAY include the query parameter _$count=true_ to tell the server to include the count of items in the response.

### 9.9 Compound collection operations
Filtering, Sorting and Pagination operations MAY all be performed against a given collection.
When these operations are performed together, the evaluation order MUST be:

1. **Filtering**. This includes all range expressions performed as an AND operation.
2. **Sorting**. The potentially filtered list is sorted according to the sort criteria.
3. **Pagination**. The materialized paginated view is presented over the filtered, sorted list. This applies to both server-driven pagination and client-driven pagination.

## 10 Delta queries
Services MAY choose to support delta queries.

### 10.1 Delta links
Delta links are opaque, service-generated links that the client uses to retrieve subsequent changes to a result.

At a conceptual level delta links are based on a defining query that describes the set of results for which changes are being tracked.
The delta link encodes the collection of entities for which changes are being tracked, along with a starting point from which to track changes.

If the query contains a filter, the response MUST include only changes to entities matching the specified criteria.
The key principles of the Delta Query are:
- Every item in the set MUST have a persistent identifier. That identifier SHOULD be represented as "id". This identifier is a service defined opaque string that MAY be used by the client to track object across calls.
- The delta MUST contain an entry for each entity that newly matches the specified criteria, and MUST contain a "@removed" entry for each entity that no longer matches the criteria.
- Re-evaluate the query and compare it to original set of results; every entry uniquely in the current set MUST be returned as an Add operation, and every entry uniquely in the original set MUST be returned as a "remove" operation.
- Each entity that previously did not match the criteria but matches it now MUST be returned as an "add"; conversely, each entity that previously matched the query but no longer does MUST be returned as a "@removed" entry.
- Entities that have changed MUST be included in the set using their standard representation.
- Services MAY add additional metadata to the "@removed" node, such as a reason for removal, or a "removed at" timestamp. We recommend teams coordinate with the Microsoft REST API Guidelines Working Group on extensions to help maintain consistency.

The delta link MUST NOT encode any client top or skip value.

### 10.2 Entity representation
Added and updated entities are represented in the entity set using their standard representation.
From the perspective of the set, there is no difference between an added or updated entity.

Removed entities are represented using only their "id" and an "@removed" node.
The presence of an "@removed" node MUST represent the removal of the entry from the set.

### 10.3 Obtaining a delta link
A delta link is obtained by querying a collection or entity and appending a $delta query string parameter.
For example:

```http
GET https://api.contoso.com/v1.0/people?$delta
HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Matt"},
    { "id": "2", "name": "Mark"},
    { "id": "3", "name": "John"},
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

Note: If the collection is paginated the deltaLink will only be present on the final page but MUST reflect any changes to the data returned across all pages.

### 10.4 Contents of a delta link response
Added/Updated entries MUST appear as regular JSON objects, with regular item properties.
Returning the added/modified items in their regular representation allows the client to merge them into their existing "cache" using standard merge concepts based on the "id" field.

Entries removed from the defined collection MUST be included in the response.
Items removed from the set MUST be represented using only their "id" and an "@removed" node.

### 10.5 Using a delta link
The client requests changes by invoking the GET method on the delta link.
The client MUST use the delta URL as is -- in other words the client MUST NOT modify the URL in any way (e.g., parsing it and adding additional query string parameters).
In this example:

```http
GET https://{opaqueUrl} HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Mat"},
    { "id": "2", "name": "Marc"},
    { "id": "3", "@removed": {} },
    { "id": "4", "name": "Luc"}
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

The results of a request against the delta link may span multiple pages but MUST be ordered by the service across all pages in such a way as to ensure a deterministic result when applied in order to the response that contained the delta link.

If no changes have occurred, the response is an empty collection that contains a delta link for subsequent changes if requested.
This delta link MAY be identical to the delta link resulting in the empty collection of changes.

If the delta link is no longer valid, the service MUST respond with _410 Gone_. The response SHOULD include a Location header that the client can use to retrieve a new baseline set of results.

## 11 JSON standardizations
### 11.1 JSON formatting standardization for primitive types
Primitive values MUST be serialized to JSON following the rules of [RFC4627][rfc-4627].

### 11.2 Guidelines for dates and times
#### 11.2.1 Producing dates
Services MUST produce dates using the `DateLiteral` format, and SHOULD use the `Iso8601Literal` format unless there are compelling reasons to do otherwise.
Services that do use the `StructuredDateLiteral` format MUST NOT produce dates using the `T` kind unless BOTH the additional precision is REQUIRED and ECMAScript clients are explicitly unsupported.
(Non-Normative statement: When deciding which particular `DateKind` to standardize on, the approximate order of preference is `E, C, U, W, O, X, I, T`.
This optimizes for ECMAScript, .NET, and C++ programmers, in that order.)

#### 11.2.2 Consuming dates
Services MUST accept dates from clients that use the same `DateLiteral` format (including the `DateKind`, if applicable) that they produce, and SHOULD accept dates using any `DateLiteral` format.

#### 11.2.3 Compatibility
Services MUST use the same `DateLiteral` format (including the same `DateKind`, if applicable) for all resources of the same type, and SHOULD use the same `DateLiteral` format (and `DateKind`, if applicable) for all resources across the entire service.

Any change to the `DateLiteral` format produced by the service (including the `DateKind`, if applicable) and any reductions in the `DateLiteral` formats (and `DateKind`, if applicable) accepted by the service MUST be treated as a breaking change.
Any widening of the `DateLiteral` formats accepted by the service is NOT considered a breaking change.

### 11.3 JSON serialization of dates and times
Round-tripping serialized dates with JSON is a hard problem.
Although ECMAScript supports literals for most built-in types, it does not define a literal format for dates.
The Web has coalesced around the [ECMAScript subset of ISO 8601 date formats (ISO 8601)][iso-8601], but there are situations where this format is not desirable.
For those cases, this document defines a JSON serialization format that can be used to unambiguously represent dates in different formats.
Other serialization formats (such as XML) could be derived from this format.

#### 11.3.1 The `DateLiteral` format 
Dates represented in JSON are serialized using the following grammar.
Informally, a `DateValue` is either an ISO 8601-formatted string or a JSON object containing two properties named `kind` and `value` that together define a point in time.
The following is not a context-free grammar; in particular, the interpretation of `DateValue` depends on the value of `DateKind`, but this minimizes the number of productions required to describe the format.

```
DateLiteral:
  Iso8601Literal
  StructuredDateLiteral

Iso8601Literal:
  A string literal as defined in http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15. Note that the full grammar for ISO 8601 (such as "basic format" without separators) is not supported.
  All dates default to UTC unless specified otherwise.

StructuredDateLiteral:
  { DateKindProperty , DateValueProperty }
  { DateValueProperty , DateKindProperty }

DateKindProperty
  "kind" : DateKind

DateKind:
  "C"            ; see below
  "E"            ; see below
  "I"            ; see below
  "O"            ; see below
  "T"            ; see below
  "U"            ; see below
  "W"            ; see below
  "X"            ; see below

DateValueProperty:
  "value" : DateValue

DateValue:
  UnsignedInteger        ; not defined here
  SignedInteger        ; not defined here
  RealNumber        ; not defined here
  Iso8601Literal        ; as above
```

#### 11.3.2 Commentary on date formatting
A `DateLiteral` using the `Iso8601Literal` production is relatively straightforward.
Here is an example of an object with a property named `creationDate` that is set to February 13, 2015, at 1:15 p.m. UTC:

```json
{ "creationDate" : "2015-02-13T13:15Z" }
```

The `StructuredDateLiteral` consists of a `DateKind` and an accompanying `DateValue` whose valid values (and their interpretation) depend on the `DateKind`. The following table describes the valid combinations and their meaning:

DateKind | DateValue       | Colloquial Name & Interpretation                                                                                                                  | More Info
-------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------
C        | UnsignedInteger | "CLR"; number of milliseconds since midnight January 1, 0001; negative values are not allowed. *See note below.*                                  | [MSDN][clr-time]
E        | SignedInteger   | "ECMAScript"; number of milliseconds since midnight, January 1, 1970.                                                                             | [ECMA International][ecmascript-time]
I        | Iso8601Literal  | "ISO 8601"; a string limited to the ECMAScript subset.                                                                                            |
O        | RealNumber      | "OLE Date"; integral part is the number of days since midnight, December 31, 1899, and fractional part is the time within the day (0.5 = midday). | [MSDN][ole-date]
T        | SignedInteger   | "Ticks"; number of ticks (100-nanosecond intervals) since midnight January 1, 1601. *See note below.*                                             | [MSDN][ticks-time]
U        | SignedInteger   | "UNIX"; number of seconds since midnight, January 1, 1970.                                                                                        | [MSDN][unix-time]
W        | SignedInteger   | "Windows"; number of milliseconds since midnight January 1, 1601. *See note below.*                                                               | [MSDN][windows-time]
X        | RealNumber      | "Excel"; as for `O` but the year 1900 is incorrectly treated as a leap year, and day 0 is "January 0 (zero)."                                     | [Microsoft Support][excel-time]

**Important note for `C` and `W` kinds:** The native CLR and Windows times are represented by 100-nanosecond "tick" values.
To interoperate with ECMAScript clients that have limited precision, _these values MUST be converted to and from milliseconds_ when (de)serialized as a `DateLiteral`.
One millisecond is equivalent to 10,000 ticks.

**Important note for `T` kind:** This kind preserves the full fidelity of the Windows native time formats (and is trivially convertible to and from the native CLR format) but is incompatible with ECMAScript clients.
Therefore, its use SHOULD be limited to only those scenarios that both require the additional precision and do not need to interoperate with ECMAScript clients.

Here is the same example of an object with a property named creationDate that is set to February 13, 2015, at 1:15 p.m. UTC, using several formats:

```json
[
  { "creationDate" : { "kind" : "O", "value" : 42048.55 } },
  { "creationDate" : { "kind" : "E", "value" : 1423862100000 } }
]
```

One of the benefits of separating the kind from the value is that once a client knows the kind used by a particular service, it can interpret the value without requiring any additional parsing.
In the common case of the value being a number, this makes coding easier for developers:

```csharp
// We know this service always gives out ECMAScript-format dates
var date = new Date(serverResponse.someObject.creationDate.value);
```

### 11.4 Durations
[Durations][wikipedia-iso8601-durations] need to be serialized in conformance with [ISO 8601][wikipedia-iso8601-durations].
Durations are "represented by the format `P[n]Y[n]M[n]DT[n]H[n]M[n]S`."
From the standard:
- P is the duration designator (historically called "period") placed at the start of the duration representation.
- Y is the year designator that follows the value for the number of years.
- M is the month designator that follows the value for the number of months.
- W is the week designator that follows the value for the number of weeks.
- D is the day designator that follows the value for the number of days.
- T is the time designator that precedes the time components of the representation.
- H is the hour designator that follows the value for the number of hours.
- M is the minute designator that follows the value for the number of minutes.
- S is the second designator that follows the value for the number of seconds.

For example, "P3Y6M4DT12H30M5S" represents a duration of "three years, six months, four days, twelve hours, thirty minutes, and five seconds."

### 11.5 Intervals
[Intervals][wikipedia-iso8601-intervals] are defined as part of [ISO 8601][wikipedia-iso8601-intervals].
- Start and end, such as "2007-03-01T13:00:00Z/2008-05-11T15:30:00Z"
- Start and duration, such as "2007-03-01T13:00:00Z/P1Y2M10DT2H30M"
- Duration and end, such as "P1Y2M10DT2H30M/2008-05-11T15:30:00Z"
- Duration only, such as "P1Y2M10DT2H30M," with additional context information

### 11.6 Repeating intervals
[Repeating Intervals][wikipedia-iso8601-repeatingintervals], as per [ISO 8601][wikipedia-iso8601-repeatingintervals], are:

> Formed by adding "R[n]/" to the beginning of an interval expression, where R is used as the letter itself and [n] is replaced by the number of repetitions.
Leaving out the value for [n] means an unbounded number of repetitions.

For example, to repeat the interval of "P1Y2M10DT2H30M" five times starting at "2008-03-01T13:00:00Z," use "R5/2008-03-01T13:00:00Z/P1Y2M10DT2H30M."

## 12 Versioning
**All APIs compliant with the Microsoft REST API Guidelines MUST support explicit versioning.** It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

### 12.1 Versioning formats
Services are versioned using a Major.Minor versioning scheme.
Services MAY opt for a "Major" only version scheme in which case the ".0" is implied and all other rules in this section apply.
Two options for specifying the version of a REST API request are supported:
- Embedded in the path of the request URL, at the end of the service root: `https://api.contoso.com/v1.0/products/users`
- As a query string parameter of the URL: `https://api.contoso.com/products/users?api-version=1.0`

Guidance for choosing between the two options is as follows:

1. Services co-located behind a DNS endpoint MUST use the same versioning mechanism.
2. In this scenario, a consistent user experience across the endpoint is paramount. The Microsoft REST API Guidelines Working Group recommends that new top-level DNS endpoints are not created without explicit conversations with your organization's leadership team.
3. Services that guarantee the stability of their REST API's URL paths, even through future versions of the API, MAY adopt the query string parameter mechanism. This means the naming and structure of the relationships described in the API cannot evolve after the API ships, even across versions with breaking changes.
4. Services that cannot ensure URL path stability across future versions MUST embed the version in the URL path.

Certain bedrock services such as Microsoft's Azure Active Directory may be exposed behind multiple endpoints.
Such services MUST support the versioning mechanisms of each endpoint, even if that means supporting multiple versioning mechanisms.

#### 12.1.1 Group versioning
Group versioning is an OPTIONAL feature that MAY be offered on services using the query string parameter mechanism.
Group versions allow for logical grouping of API endpoints under a common versioning moniker.
This allows developers to look up a single version number and use it across multiple endpoints.
Group version numbers are well known, and services SHOULD reject any unrecognized values.

Internally, services will take a Group Version and map it to the appropriate Major.Minor version.

The Group Version format is defined as YYYY-MM-DD, for example 2012-12-07 for December 7, 2012. This Date versioning format applies only to Group Versions and SHOULD NOT be used as an alternative to Major.Minor versioning.

##### 12.1.1.1 Examples of group versioning

| Group      | Major.Minor |
|:-----------|:------------|
| 2012-12-01 | 1.0         |
|            | 1.1         |
|            | 1.2         |
| 2013-03-21 | 1.0         |
|            | 2.0         |
|            | 3.0         |
|            | 3.1         |
|            | 3.2         |
|            | 3.3         |

Version Format                | Example                | Interpretation
----------------------------- | ---------------------- | ------------------------------------------
{groupVersion}                | 2013-03-21, 2012-12-01 | 3.3, 1.2
{majorVersion}                | 3                      | 3.0
{majorVersion}.{minorVersion} | 1.2                    | 1.2

Clients can specify either the group version or the Major.Minor version:

For example:

```http
GET http://api.contoso.com/acct1/c1/blob2?api-version=1.0
```

```http
PUT http://api.contoso.com/acct1/c1/b2?api-version=2011-12-07
```

### 12.2 When to version
Services MUST increment their version number in response to any breaking API change.
See the following section for a detailed discussion of what constitutes a breaking change.
Services MAY increment their version number for nonbreaking changes as well, if desired.

Use a new major version number to signal that support for existing clients will be deprecated in the future.
When introducing a new major version, services MUST provide a clear upgrade path for existing clients and develop a plan for deprecation that is consistent with their business group's policies.
Services SHOULD use a new minor version number for all other changes.

Online documentation of versioned services MUST indicate the current support status of each previous API version and provide a path to the latest version.

### 12.3 Definition of a breaking change
Changes to the contract of an API are considered a breaking change.
Changes that impact the backwards compatibility of an API are a breaking change.

Teams MAY define backwards compatibility as their business needs require.
For example, Azure defines the addition of a new JSON field in a response to be not backwards compatible.
Office 365 has a looser definition of backwards compatibility and allows JSON fields to be added to responses.

Clear examples of breaking changes:

1. Removing or renaming APIs or API parameters
2. Changes in behavior for an existing API
3. Changes in Error Codes and Fault Contracts
4. Anything that would violate the [Principle of Least Astonishment][principle-of-least-astonishment]

Services MUST explicitly define their definition of a breaking change, especially with regard to adding new fields to JSON responses and adding new API arguments with default fields.
Services that are co-located behind a DNS Endpoint with other services MUST be consistent in defining contract extensibility.

The applicable changes described [in this section of the OData V4 spec][odata-breaking-changes] SHOULD be considered part of the minimum bar that all services MUST consider a breaking change.

## 13 Long running operations
Long running operations, sometimes called async operations, tend to mean different things to different people.
This section sets forth guidance around different types of long running operations, and describes the wire protocols and best practices for these types of operations.

1. One or more clients MUST be able to monitor and operate on the same resource at the same time.
2. The state of the system SHOULD be discoverable and testable at all times. Clients SHOULD be able to determine the system state even if the operation tracking resource is no longer active. The act of querying the state of a long running operation should itself leverage principles of the web. i.e. well defined resources with uniform interface semantics. Clients MAY issue a GET on some resource to determine the state of a long running operation
3. Long running operations SHOULD work for clients looking to "Fire and Forget" and for clients looking to actively monitor and act upon results.
4. Cancellation does not explicitly mean rollback. On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc. Following a cancelled operation, It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

### 13.1 Resource based long running operations (RELO)
Resource based modeling is where the status of an operation is encoded in the resource and the wire protocol used is the standard synchronous protocol.
In this model state transitions are well defined and goal states are similarly defined.

_This is the preferred model for long running operations and should be used wherever possible._ Avoiding the complexity and mechanics of the LRO Wire Protocol makes things simpler for our users and tooling chain.

An example may be a machine reboot, where the operation itself completes synchronously but the GET operation on the virtual machine resource would have a "state: Rebooting", "state: Running" that could be queried at any time.

This model MAY integrate Push Notifications.

While most operations are likely to be POST semantics, In addition to POST semantics services MAY support PUT semantics via routing to simplify their APIs.
For example, a user that wants to create a database named "db1" could call:

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the databases segment is processing the PUT operation.

Services MAY also use the hybrid defined below.

### 13.2 Stepwise long running operations
A stepwise operation is one that takes a long, and often unpredictable, length of time to complete, and doesn't offer state transition modeled in the resource.
This section outlines the approach that services should use to expose such long running operations.

Service MAY expose stepwise operations.

> Stepwise Long Running Operations are sometimes called "Async" operations.
This causes confusion, as it mixes elements of platforms ("Async / await", "promises", "futures") with elements of API operation.
This document uses the term "Stepwise Long Running Operation" or often just "Stepwise Operation" to avoid confusion over the word "Async".

Services MUST perform as much synchronous validation as practical on stepwise requests.
Services MUST prioritize returning errors in a synchronous way, with the goal of having only "Valid" operations processed using the long running operation wire protocol.

For an API that's defined as a Stepwise Long Running Operation the service MUST go through the Stepwise Long Running Operation flow even if the operation can be completed immediately.
In other words, APIs must adopt and stick with a LRO pattern and not change patterns based on circumstance.

#### 13.2.1 PUT
Services MAY enable PUT requests for entity creation.

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the _databases_ segment is processing the PUT operation.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

For services that need to return a 201 Created here, use the hybrid flow described below.

The 202 Accepted should return no body.
The 201 Created case should return the body of the target resource.

#### 13.2.2 POST
Services MAY enable POST requests for entity creation.

```http
POST https://api.contoso.com/v1.0/databases/

{
  "fileName": "someFile.db",
  "color": "red"
}
```

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

#### 13.2.3 POST, hybrid model
Services MAY respond synchronously to POST requests to collections that create a resource even if the resources aren't fully created when the response is generated.
In order to use this pattern, the response MUST include a representation of the incomplete resource and an indication that it is incomplete.

For example:

```http
POST https://api.contoso.com/v1.0/databases/ HTTP/1.1
Host: api.contoso.com
Content-Type: application/json
Accept: application/json

{
  "fileName": "someFile.db",
  "color": "red"
}
```

Service response says the database has been created, but indicates the request is not completed by including the Operation-Location header.
In this case the status property in the response payload also indicates the operation has not fully completed.

```http
HTTP/1.1 201 Created
Location: https://api.contoso.com/v1.0/databases/db1
Operation-Location: https://api.contoso.com/v1.0/operations/123

{
  "databaseName": "db1",
  "color": "red",
  "Status": "Provisioning",
  [ … other fields for "database" …]
}
```

#### 13.2.4 Operations resource
Services MAY provide a "/operations" resource at the tenant level.

Services that provide the "/operations" resource MUST provide GET semantics.
GET MUST enumerate the set of operations, following standard pagination, sorting, and filtering semantics.
The default sort order for this operation MUST be:

Primary Sort           | Secondary Sort
---------------------- | -----------------------
Not Started Operations | Operation Creation Time
Running Operations     | Operation Creation Time
Completed Operations   | Operation Creation Time

Note that "Completed Operations" is a goal state (see below), and may actually be any of several different states such as "successful", "cancelled", "failed" and so forth.

#### 13.2.5    Operation resource
An operation is a user addressable resource that tracks a stepwise long running operation.
Operations MUST support GET semantics.
The GET operation against an operation MUST return:

1. The operation resource, it's state, and any extended state relevant to the particular API.
2. 200 OK as the response code.

Services MAY support operation cancellation by exposing DELETE on the operation.
If supported DELETE operations MUST be idempotent.

> Note: From an API design perspective, cancellation does not explicitly mean rollback.
On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc.
Following a cancelled operation It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

Services that do not support operation cancellation MUST return a 405 Method Not Allowed in the event of a DELETE.

Operations MUST support the following states:

1. NotStarted
2. Running
3. Succeeded. Terminal State.
4. Failed. Terminal State.

Services MAY add additional states, such as "Cancelled" or "Partially Completed". Services that support cancellation MUST sufficiently describe their cancellation such that the state of the system can be accurately determined and any compensating actions may be run.

Services that support additional states should consider this list of canonical names and avoid creating new names if possible: Cancelling, Cancelled, Aborting, Aborted, Tombstone, Deleting, Deleted.

An operation MUST contain, and provide in the GET response, the following information:

1. The timestamp when the operation was created.
2. A timestamp for when the current state was entered.
3. The operation state (notstarted / running / completed).

Services MAY add additional, API specific, fields into the operation.
The operation status JSON returned looks like:

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-01-03.45Z",
  "status": "notstarted | running | succeeded | failed"
}
```

##### 13.2.5.1 Percent complete
Sometimes it is impossible for services to know with any accuracy when an operation will complete.
Which makes using the Retry-After header problematic.
In that case, services MAY include, in the operationStatus JSON, a percent complete field.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "percentComplete": "50",
  "status": "running"
}
```

In this example the server has indicated to the client that the long running operation is 50% complete.

##### 13.2.5.2 Target resource location
For operations that result in, or manipulate, a resource the service MUST include the target resource location in the status upon operation completion.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.6 Operation tombstones
Services MAY choose to support tombstoned operations.
Services MAY choose to delete tombstones after a service defined period of time.

#### 13.2.7 The typical flow, polling
- Client invokes a stepwise operation by invoking an action using POST
- The server MUST indicate the request has been started by responding with a 202 Accepted status code. The response SHOULD include the location header containing a URL that the client should poll for the results after waiting the number of seconds specified in the Retry-After header.
- Client polls the location until receiving a 200 OK response from the server.

##### 13.2.7.1 Example of the typical flow, polling
Client invokes the restart action:

```http
POST https://api.contoso.com/v1.0/databases HTTP/1.1
Accept: application/json

{
  "fromFile": "myFile.db",
  "color": "red"
}
```

The server response indicates the request has been created.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

Client waits for a period of time then invokes another request to try to get the operation status.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds that results are still not ready and optionally provides a recommendation to wait 30 seconds.

```http
HTTP/1.1 200 OK
Retry-After: 30

{
  "createdDateTime": "2015-06-19T12-01-03.4Z",
  "status": "running"
}
```

Client waits the recommended 30 seconds and then invokes another request to get the results of the operation.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds with a "status:succeeded" operation that includes the resource location.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.8 The typical flow, push notifications
1. Client invokes a long running operation by invoking an action using POST. The client has a push notification already setup on the parent resource.
2. The service indicates the request has been started by responding with a 202 Accepted status code. The client ignores everything else.
3. Upon completion of the overall operation the service pushes a notification via the subscription on the parent resource.
4. The client retrieves the operation result via the resource URL.

##### 13.2.8.1 Example of the typical flow, push notifications existing subscription
Client invokes the backup action.
The client already has a push notification subscription setup for db1.

```http
POST https://api.contoso.com/v1.0/databases/db1?backup HTTP/1.1
Accept: application/json
```

The server response indicates the request has been accepted.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

The caller ignores all the headers in the return.

The target URL receives a push notification when the operation is complete.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "value": [
    {
      "subscriptionId": "1234-5678-1111-2222",
      "context": "subscription context that was specified at setup",
      "resourceUrl": "https://api.contoso.com/v1.0/databases/db1",
      "userId" : "contoso.com/user@contoso.com",
      "tenantId" : "contoso.com"
    }
  ]
}
```

#### 13.2.9 Retry-After
In the examples above the Retry-After header indicates the number of seconds that the client should wait before trying to get the result from the URL identified by the location header.

The HTTP specification allows the Retry-After header to alternatively specify a HTTP date, so clients should be prepared to handle this as well.

```http
HTTP/1.1 202 Accepted
Operation-Location: http://api.contoso.com/v1.0/operations/123
Retry-After: 60
```

Note: The use of the HTTP Date is inconsistent with the use of ISO 8601 Date Format used throughout this document, but is explicitly defined by the HTTP standard in [RFC 7231][rfc-7231-7-1-1-1]. Services SHOULD prefer the integer number of seconds (in decimal) format over the HTTP date format.

### 13.3 Retention policy for operation results
In some situations, the result of a long running operation is not a resource that can be addressed.
For example, if you invoke a long running Action that returns a Boolean (rather than a resource).
In these situations, the Location header points to a place where the Boolean result can be retrieved.

Which begs the question: "How long should operation results be retained?"

A recommended minimum retention time is 24 hours.

Operations SHOULD transition to "tombstone" for an additional period of time prior to being purged from the system.

## 14 Push notifications via webhooks
### 14.1 Scope
Services MAY implement push notifications via web hooks.
This section addresses the following key scenario:

> Push notification via HTTP Callbacks, often called Web Hooks, to publicly-addressable servers.

The approach set forth is chosen due to its simplicity, broad applicability, and low barrier to entry for service subscribers.
It's intended as a minimal set of requirements and as a starting point for additional functionality.

### 14.2 Principles
The core principles for services that support web hooks are:

1. Services MUST implement at least a poke/pull model. In the poke/pull model, a notification is sent to a client, and clients then send a request to get the current state or the record of change since their last notification. This approach avoids complexities around message ordering, missed messages, and change sets.  Services MAY add more data to provide rich notifications.
2. Services MUST implement the challenge/response protocol for configuring callback URLs.
3. Services SHOULD have a recommended age-out period, with flexibility for services to vary based on scenario.
4. Services SHOULD allow subscriptions that are raising successful notifications to live forever and SHOULD be tolerant of reasonable outage periods.
5. Firehose subscriptions MUST be delivered only over HTTPS. Services SHOULD require other subscription types to be HTTPS. See the "Security" section for more details.

### 14.3 Types of subscriptions
There are two subscription types, and services MAY implement either, both, or none.
The supported subscription types are:

1. Firehose subscriptions – a subscription is manually created for the subscribing application, typically in an app registration portal.  Notifications of activity that any users have consented to the app receiving are sent to this single subscription.
2. Per-resource subscriptions – the subscribing application uses code to programmatically create a subscription at runtime for some user-specific entity(s).

Services that support both subscription types SHOULD provide differentiated developer experiences for the two types:

1. Firehose – Services MUST NOT require developers to create code except to directly verify and respond to notifications.  Services MUST provide administrative UI for subscription management.  Services SHOULD NOT assume that end users are aware of the subscription, only the subscribing application's functionality.
2. Per-user – Services MUST provide an API for developers to create and manage subscriptions as part of their app as well as verifying and responding to notifications.  Services MAY expect end users to be aware of subscriptions and MUST allow end users to revoke subscriptions where they were created directly in response to user actions.

### 14.4 Call sequences
The call sequence for a firehose subscription MUST follow the diagram below.
It shows manual registration of application and subscription, and then the end user making use of one of the service's APIs.
At this part of the flow, two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope.)
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.

The final part of the sequence is the notification flow itself.

Non-normative implementation guidance:  A resource in the service changes and the service needs to run the following logic:

1. Determine the set of users who have access to the resource, and could thus expect apps to receive notifications about it on their behalf.
2. See which of those users have consented to receiving notifications and from which apps.
3. See which apps have registered a firehose subscription.
4. Join 1, 2, 3 to produce the concrete set of notifications that must be sent to apps.

It should be noted that the act of user consent and the act of setting up a firehose subscription could arrive in either order.
Services SHOULD send notifications with setup processed in either order.

![Firehose subscription setup][websequencediagram-firehose-subscription-setup]

For a per-user subscription, app registration is either manual or automated.
The call flow for a per-user subscription MUST follow the diagram below.
It shows the end user making use of one of the service's APIs, and again, the same two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope).   
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.  

In this case, the subscription is set up programmatically using the end-user's token from the subscribing application.
The app MUST store the ID of the registered subscription alongside the user tokens.

Non normative implementation guidance: In the final part of the sequence, when an item of data in the service changes and the service needs to run the following logic:

1. Find the set of subscriptions that correspond via resource to the data that changed.
2. For subscriptions created under an app+user token, send a notification to the app per subscription with the subscription ID and user id of the subscription-creator.
- For subscriptions created with an app only token, check that the owner of the changed data or any user that has visibility of the changed data has consented to notifications to the application, and if so send a set of notifications per user id to the app per subscription with the subscription ID.

  ![User subscription setup][websequencediagram-user-subscription-setup]

### 14.5 Verifying subscriptions
When subscriptions change either programmatically or in response to change via administrative UI portals, the subscribing service needs to be protected from malicious or unexpected calls from services pushing potentially large volumes of notification traffic.

For all subscriptions, whether firehose or per-user, services MUST send a verification request as part of creation or modification via portal UI or API request, before sending any other notifications.

Verification requests MUST be of the following format as an HTTP/HTTPS POST to the subscription's _notificationUrl_.

```http
POST https://{notificationUrl}?validationToken={randomString}
ClientState: clientOriginatedOpaqueToken (if provided by client on subscription-creation)
Content-Length: 0
```

For the subscription to be set up, the application MUST respond with 200 OK to this request, with the _validationToken_ value as the sole entity body.
Note that if the _notificationUrl_ contains query parameters, the _validationToken_ parameter must be appended with an `&`.

If any challenge request does not receive the prescribed response within 5 seconds of sending the request, the service MUST return an error, MUST NOT create the subscription, and MUST NOT send further requests or notifications to _notificationUrl_.

Services MAY perform additional validations on URL ownership.

### 14.6 Receiving notifications
Services SHOULD send notifications in response to service data changes that do not include details of the changes themselves, but include enough information for the subscribing application to respond appropriately to the following process:

1. Applications MUST identify the correct cached OAuth token to use for a callback
2. Applications MAY look up any previous delta token for the relevant scope of change
3. Applications MUST determine the URL to call to perform the relevant query for the new state of the service, which MAY be a delta query.

Services that are providing notifications that will be relayed to end users MAY choose to add more detail to notification packets in order to reduce incoming call load on their service.
 Such services MUST be clear that notifications are not guaranteed to be delivered and may be lossy or out of order.

Notifications MAY be aggregated and sent in batches.
Applications MUST be prepared to receive multiple events inside a single push notification.

The service MUST send all Web Hook data notifications as POST requests.

Services MUST allow for a 30-second timeout for notifications.
If a timeout occurs or the application responds with a 5xx response, then the service SHOULD retry the notification with exponential back-off.
All other responses will be ignored.

The service MUST NOT follow 301/302 redirect requests.

#### 14.6.1 Notification payload
The basic format for notification payloads is a list of events, each containing the id of the subscription whose referenced resources have changed, the type of change, the resource that should be consumed to identify the exact details of the change and sufficient identity information to look up the token required to call that resource.

For a firehose subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab18b",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files?$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

For a per-user subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab183",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    },
    {
      "subscriptionId": "97b391179fa22",
      "clientState ": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

Following is a detailed description of the JSON payload.

A notification item consists a top-level object that contains an array of events, each of which identified the subscription due to which this notification is being sent.

Field | Description
----- | --------------------------------------------------------------------------------------------------
value | Array of events that have been raised within the subscription’s scope since the last notification.

Each item of the events array contains the following properties:

Field              | Description
------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
subscriptionId     | The id of the subscription due to which this notification has been sent.<br/>Services MUST provide the *subscriptionId* field.
clientState        | Services MUST provide the *clientState* field if it was provided at subscription creation time.
expirationDateTime | Services MUST provide the *expirationDateTime* field if the subscription has one.
resource           | Services MUST provide the resource field. This URL MUST be considered opaque by the subscribing application.  In the case of a richer notification it MAY be subsumed by message content that implicitly contains the resource URL to avoid duplication.<br/>If a service is providing this data as part of a more detailed data packet, then it need not be duplicated.
userId             | Services MUST provide this field for user-scoped resources.  In the case of user-scoped resources, the unique identifier for the user should be used.<br/>In the case of resources shared between a specific set of users, multiple notifications must be sent, passing the unique identifier of each user.<br/>For tenant-scoped resources, the user id of the subscription should be used.
tenantId           | Services that wish to support cross-tenant requests SHOULD provide this field. Services that provide notifications on tenant-scoped data MUST send this field.

### 14.7 Managing subscriptions programmatically
For per-user subscriptions, an API MUST be provided to create and manage subscriptions.
The API must support at least the operations described here.

#### 14.7.1 Creating subscriptions
A client creates a subscription by issuing a POST request against the subscriptions resource.
The subscription namespace is client-defined via the POST operation.

```
https://api.contoso.com/apiVersion/$subscriptions
```

The POST request contains a single subscription object to be created.
That subscription object has the following properties:

Property Name   | Required | Notes
--------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------
resource        | Yes      | Resource path to watch.
notificationUrl | Yes      | The target web hook URL.
clientState     | No       | Opaque string passed back to the client on all notifications. Callers may choose to use this to provide tagging mechanisms.

If the subscription was successfully created, the service MUST respond with the status code 201 CREATED and a body containing at least the following properties:

Property Name      | Required | Notes
------------------ | -------- | -------------------------------------------------------------------------------------------
id                 | Yes      | Unique ID of the new subscription that can be used later to update/delete the subscription.
expirationDateTime | No       | Uses existing Microsoft REST API Guidelines defined time formats.

Creation of subscriptions SHOULD be idempotent.
The combination of properties scoped to the auth token, provides a uniqueness constraint.

Below is an example request using a User + Application principal to subscribe to notifications from a file:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "resource": "http://api.service.com/v1.0/files/file1.txt",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "32b8cbd6174ab18b",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

Below is an example using an Application-Only principal where the application is watching all files to which it's authorized:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {ApplicationPrincipalBearerToken}

{
  "resource": "All.Files",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "8cbd6174abb391179",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

#### 14.7.2 Updating subscriptions
Services MAY support amending subscriptions.
 To update the properties of an existing subscription, clients use PATCH requests providing the ID and the properties that need to change.
Omitted properties will retain their values.
To delete a property, assign a value of JSON null to it.

As with creation, subscriptions are individually managed.

The following request changes the notification URL of an existing subscription:

```http
PATCH https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "notificationUrl": "https://contoso.com/myNewCallback"
}
```

If the PATCH request contains a new _notificationUrl_, the server MUST perform validation on it as described above.
If the new URL fails to validate, the service MUST fail the PATCH request and leave the subscription in its previous state.

The service MUST return an empty body and `204 No Content` to indicate a successful patch.

The service MUST return an error body and status code if the patch failed.

The operation MUST succeed or fail atomically.

#### 14.7.3 Deleting subscriptions
Services MUST support deleting subscriptions.
Existing subscriptions can be deleted by making a DELETE request against the subscription resource:

```http
DELETE https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

As with update, the service MUST return `204 No Content` for a successful delete, or an error body and status code to indicate failure.

#### 14.7.4 Enumerating subscriptions
To get a list of active subscriptions, clients issue a GET request against the subscriptions resource using a User + Application or Application-Only bearer token:

```http
GET https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

The service MUST return a format as below using a User + Application principal bearer token:

```json
{
  "value": [
    {
      "id": "32b8cbd6174ab18b",
      "resource": " http://api.contoso.com/v1.0/files/file1.txt",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

An example that may be returned using Application-Only principal bearer token:

```json
{
  "value": [
    {
      "id": "6174ab18bfa22",
      "resource": "All.Files ",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

### 14.8 Security
All service URLs must be HTTPS (that is, all inbound calls MUST be HTTPS). Services that deal with Web Hooks MUST accept HTTPS.

We recommend that services that allow client defined Web Hook Callback URLs SHOULD NOT transmit data over HTTP.
This is because information can be inadvertently exposed via client, network, server logs and other mechanisms.

However, there are scenarios where the above recommendations cannot be followed due to client endpoint or software limitations.
Consequently, services MAY allow web hook URLs that are HTTP.

Furthermore, services that allow client defined HTTP web hooks callback URLs SHOULD be compliant with privacy policy specified by engineering leadership.
This will typically include recommending that clients prefer SSL connections and adhere to special precautions to ensure that logs and other service data collection are properly handled.

For example, services may not want to require developers to generate certificates to onboard.
Services might only enable this on test accounts.

## 15 Unsupported requests
RESTful API clients MAY request functionality that is currently unsupported.
RESTful APIs MUST respond to valid but unsupported requests consistent with this section.

### 15.1 Essential guidance
RESTful APIs will often choose to limit functionality that can be performed by clients.
For instance, auditing systems allow records to be created but not modified or deleted.
Similarly, some APIs will expose collections but require or otherwise limit filtering and ordering criteria, or MAY not support client-driven pagination.

### 15.2 Feature allow list
If a service does not support any of the below API features, then an error response MUST be provided if the feature is requested by a caller.
The features are:
- Key Addressing in a collection, such as: `https://api.contoso.com/v1.0/people/user1@contoso.com`
- Filtering a collection by a property value, such as: `https://api.contoso.com/v1.0/people?$filter=name eq 'david'`
- Filtering a collection by range, such as: `http://api.contoso.com/v1.0/people?$filter=hireDate ge 2014-01-01 and hireDate le 2014-12-31`
- Client-driven pagination via $top and $skip, such as: `http://api.contoso.com/v1.0/people?$top=5&$skip=2`
- Sorting by $orderBy, such as: `https://api.contoso.com/v1.0/people?$orderBy=name desc`
- Providing $delta tokens, such as: `https://api.contoso.com/v1.0/people?$delta`

#### 15.2.1 Error response
Services MUST provide an error response if a caller requests an unsupported feature found in the feature allow list.
The error response MUST be an HTTP status code from the 4xx series, indicating that the request cannot be fulfilled.
Unless a more specific error status is appropriate for the given request, services SHOULD return "400 Bad Request" and an error payload conforming to the error response guidance provided in the Microsoft REST API Guidelines.
Services SHOULD include enough detail in the response message for a developer to determine exactly what portion of the request is not supported.

Example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name HTTP/1.1
Accept: application/json
```

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": {
    "code": "ErrorUnsupportedOrderBy",
    "message": "Ordering by name is not supported."
  }
}
```

## 16 Naming guidelines
### 16.1 Approach
Naming policies should aid developers in discovering functionality without having to constantly refer to documentation.
Use of common patterns and standard conventions greatly aids developers in correctly guessing common property names and meanings.
Services SHOULD use verbose naming patterns and SHOULD NOT use abbreviations other than acronyms that are the dominant mode of expression in the domain being represented by the API, (e.g. Url).

### 16.2 Casing
- Acronyms SHOULD follow the casing conventions as though they were regular words (e.g. Url).
- All identifiers including namespaces, entityTypes, entitySets, properties, actions, functions and enumeration values SHOULD use lowerCamelCase.
- HTTP headers are the exception and SHOULD use standard HTTP convention of Capitalized-Hyphenated-Terms.

### 16.3 Names to avoid
Certain names are so overloaded in API domains that they lose all meaning or clash with other common usages in domains that cannot be avoided when using REST APIs, such as OAUTH.
Services SHOULD NOT use the following names:
- Context
- Scope
- Resource

### 16.4 Forming compound names
- Services SHOULD avoid using articles such as 'a', 'the', 'of' unless needed to convey meaning.
	- e.g. names such as aUser, theAccount, countOfBooks SHOULD NOT be used, rather user, account, bookCount SHOULD be preferred.
- Services SHOULD add a type to a property name when not doing so would cause ambiguity about how the data is represented or would cause the service not to use a common property name.
- When adding a type to a property name, services MUST add the type at the end, e.g. createdDateTime.

### 16.5 Identity properties
- Services MUST use string types for identity properties.
- For OData services, the service MUST use the OData @id property to represent the canonical identifier of the resource.
- Services MAY use the simple 'id' property to represent a local or legacy primary key value for a resource.
- Services SHOULD use the name of the relationship postfixed with 'Id' to represent a foreign key to another resource, e.g. subscriptionId.
	- The content of this property SHOULD be the canonical ID of the referenced resource.

### 16.6 Date and time properties

- For properties requiring both date and time, services MUST use the suffix 'DateTime'.
- For properties requiring only date information without specifying time, services MUST use the suffix 'Date', e.g. birthDate.
- For properties requiring only time information without specifying date, services MUST use the suffix 'Time', e.g. appointmentStartTime.

### 16.7 Name properties
- For the overall name of a resource typically shown to users, services MUST use the property name 'displayName'.
- Services MAY use other common naming properties, e.g. givenName, surname, signInName.

### 16.8 Collections and counts
- Services MUST name collections as plural nouns or plural noun phrases using correct English.
- Services MAY use simplified English for nouns that have plurals not in common verbal usage.
	- e.g. schemas MAY be used instead of schemata.
- Services MUST name counts of resources with a noun or noun phrase suffixed with 'Count'.

### 16.9 Common property names
Where services have a property whose data matches the names below, the service MUST use the name from this table.
This table will grow as services add terms that will be more commonly used.
Service owners adding such terms SHOULD propose additions to this document.

| |
|------------- |
 attendees     |
 body          |  
 createdDateTime |
 childCount    |  
 children      |  
 contentUrl    |  
 country       |  
 createdBy     |  
 displayName   |
 errorUrl      |
 eTag          |
 event         |
 expirationDateTime |
 givenName     |
 jobTitle      |
 kind          |  
 id            |
 lastModifiedDateTime |
 location      |
 memberOf      |
 message       |
 name          |  
 owner         |
 people        |  
 person        |  
 postalCode    |
 photo         |
 preferredLanguage |
 properties    |
 signInName    |
 surname       |
 tags          |
 userPrincipalName |
 webUrl        |

## 17 Appendix
### 17.1 Sequence diagram notes
All sequence diagrams in this document are generated using the WebSequenceDiagrams.com web site. To generate them, paste the text below into the web tool.

#### 17.1.1 Push notifications, per user flow

```
=== Being Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual App Registration


Developer <--> DB Portal : Login into Portal, App Registration UX
DB Portal -> +DB App Registration: App Name etc.
note over DB App Registration: Confirm Portal Access Token

DB App Registration -> -DB Portal: App ID
DB Portal <--> App Server: Developer copies App ID

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual Notification Registration

Developer <--> DB Portal: webhook registration UX
DB Portal -> +DB Notifications: Register: App Server webhook URL, Scope, App ID
Note over DB Notifications : Confirm Portal Access Token
DB Notifications -> -DB Portal: notification ID
DB Portal --> App Server : Developer may copy notification ID


note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token
=== End Text ===
```

#### 17.1.2 Push notifications, firehose flow

```
=== Being Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : App Registration

alt Automated app registration
   Developer <--> Automation: Configure
   Automation -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm App Access Token
   DB App Registration -> -Automation: App ID, App Secret
   Automation --> App Server : Embed App ID, App Secret
else Manual app registration
   Developer <--> DB Portal : Login into Portal, App Registration UX
   DB Portal -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm Portal Access Token

   DB App Registration -> -DB Portal: App ID
   DB Portal <--> App Server: Developer copies App ID
end

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Registration

App Server->+DB Notifications: Register: App server webhook URL, Scope, App ID
note over DB Notifications : Confirm User Access Token
DB Notifications -> -App Server: notification ID
note right of App Server : Cache the Notification ID and User Access Token



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token



=== End Text ===
```
[fielding]: https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
[IANA-headers]: http://www.iana.org/assignments/message-headers/message-headers.xhtml
[rfc7231-7-1-1-1]: https://tools.ietf.org/html/rfc7231#section-7.1.1.1
[rfc-7230-3-1-1]: https://tools.ietf.org/html/rfc7230#section-3.1.1
[rfc-7231]: https://tools.ietf.org/html/rfc7231
[rest-in-practice]: http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829/
[rest-on-wikipedia]: http://en.wikipedia.org/wiki/Representational_state_transfer
[rfc-5789]: http://tools.ietf.org/html/rfc5789
[rfc-5988]: http://tools.ietf.org/html/rfc5988
[rfc-3339]: https://tools.ietf.org/html/rfc3339
[rfc-5322-3-3]: https://tools.ietf.org/html/rfc5322#section-3.3
[cors-preflight]: http://www.w3.org/TR/cors/#resource-preflight-requests
[rfc-3864]: http://www.ietf.org/rfc/rfc3864.txt
[odata-json-annotations]: http://docs.oasis-open.org/odata/odata-json-format/v4.0/os/odata-json-format-v4.0-os.html#_Instance_Annotations
[cors]: http://www.w3.org/TR/access-control/
[cors-user-credentials]: http://www.w3.org/TR/access-control/#user-credentials
[cors-simple-headers]: http://www.w3.org/TR/access-control/#simple-header
[rfc-4627]: https://tools.ietf.org/html/rfc4627
[iso-8601]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15
[clr-time]: https://msdn.microsoft.com/en-us/library/System.DateTime(v=vs.110).aspx
[ecmascript-time]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1
[ole-date]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms221199(v=vs.85).aspx
[ticks-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[unix-time]: https://msdn.microsoft.com/en-us/library/1f4c8f33.aspx
[windows-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[excel-time]: http://support.microsoft.com/kb/214326?wa=wsignin1.0
[wikipedia-iso8601-durations]: http://en.wikipedia.org/wiki/ISO_8601#Durations
[wikipedia-iso8601-intervals]: http://en.wikipedia.org/wiki/ISO_8601#Time_intervals
[wikipedia-iso8601-repeatingintervals]: http://en.wikipedia.org/wiki/ISO_8601#Repeating_intervals
[principle-of-least-astonishment]: http://en.wikipedia.org/wiki/Principle_of_least_astonishment
[odata-breaking-changes]: http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_Toc406398209
[websequencediagram-firehose-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDogTWFudWFsAIFzEQoKCgCDAgo8LS0-AIIqCiA6IExvZ2luIGludG8Agj8JAII1ECBVWCAKACoKLT4gKwCCWBM6AIQGBU5hbWUgZXRjLgCDFQ4AGxJDb25maXJtAIEBCEFjY2VzcyBUb2tlbgoKAIM3EyAtPiAtAINkCQBnBklEAIEMCwCBVQUAhQIMAIR3CmNvcGllcwArCACCIHAAhHMMAIMKDwCDABg6IHdlYmhvb2sgcgCCeg4AgnUSAIVQDToAhXYHZXIAgwgGAIcTBgBECVVSTCwgU2NvcGUAhzIGSUQKTgCGPQwAhhwNIACDBh4AHhEAgxEPbgCBagwAgxwNAIMaDiAAgx0MbWF5IGNvcHkALREAhVtqAIZHB0F1dGhvcml6AIY7BwCGXQctPiArAIEuDVJlcXVlc3QgYQCFOQZ0byBEQiBwcm90ZWN0ZWQgaW5mb3IAiiQGCgCDBQstPiAtAIctCVJlZGlyZWN0ADYHAGwNIGVuZHBvaW50AIoWBmEADw1yAHYGAIEQDACJVAcASwtlZAAYHgCICAgAMAcAcA4AhGoGAE0FAIEdFmJhY2sgdG8AhF8NaXRoIGNvZGUAghoaaQCBagcAgToHAD0JAII-B3MAPgsAglEHAEsFAIIzDgCBXw0Agn8GdG9rZW5zACcSAI0_BXJpZ2h0IG9mAItpDUNhY2hlIHRoYXQgdGhpcyBVc2VyIElEIHByb3ZpZGVkAINNCwCIZgoAggcJAIN7D3Nwb25zAI0_BwCECgYsIHJlZnJlc2gsIGFuZCBJRACBHAcAgQMPAIYADQCBDAcAgUUGYnkAjFkFIElEAIQkG3R1cm4AhF4MIHRvIGMAjR8FAIwRagCJVw1GbG93AIYqCQCMaQgAgmoKaGFuZ2UAj3YFAIFXBWRhdGEgLSB0eXBpY2FsIHZpYQCQDgVyYWN0aW5nAJAPBgCJQQt2aWEAjnsHCgCPNgogAIhDEACKZw0AkFMFAIkBDwCDDAUAgkYWKwBNCwCHWApjAIEyBQCHRg0AhWUHYWNoAIQeDACEfwVhbmQgInNpbmNlIgCFEQYAkSQOAIR3CgCNfwcAhHQFAIpQEACBUgsAhFAcAII8BWFuZCBuZXcAYRQAhFUTOiBVcGRhdGUgc3RhdHUAgSkGAIFDBQAxEwoKCg&s=mscgen
[websequencediagram-user-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDoAgWwRCgphbHQAgyUIAIEHBiByABQMICAAgxsLPC0tPgCDTws6IENvbmZpZ3VyZQogIACDaAsgLT4gKwCCWBMAegZOYW1lIGV0Yy4AhAgFAIMaDQAfEgBdBXJtAIQ_BUFjY2VzcyBUb2tlAIETBgCDOxIgLT4gLQCBFgxBcHAgSUQAhHwIY3JldACBGxAtPgCFFgsgOiBFbWJlZAAkFGVsc2UgTWFudWFsAIIEJACEbQkgOiBMb2dpbiBpbnRvAIUBCQCBKRFVWACGGAUALQoAgh8mAIIZKwCBCAcAgjoNAIIsHACGLwkAgj8IAIESDgCECAYAh1ELAIdFCmNvcGllcwAuCGVuZACEeGoAhWQHQXV0aG9yaXoAhV8HAIV6By0-ICsAg2ANUmVxdWVzdCBhAIRVBnRvIERCIHByb3RlY3RlZCBpbmZvcgCJQQYKAIQaCy0-IC0AhkoJUmVkaXJlY3QANgcAbA0gZW5kcG9pbnQAiTMGYQAPDXIAdgYAgRAMAIhxBwBLC2VkABgeAIRjCAAwB0EAcQxVWAoASQgAgRwWYmFjayB0bwCFdAwAilwFY29kZQCCGRppAIFpBwCBOQcAPQkAgj0HcwA-CwCCUAcASwUAgjIOAIFeDQCCfgZ0b2tlbnMAJxIAjFsFcmlnaHQgb2YAiwUNQ2FjaGUgdGhhdCB0aGlzIFVzZXIgSUQgcHJvdmlkZWQAg0wLAIU6BwCCBAwAg3oPc3BvbnMAjFsHAIQJBiwgcmVmcmVzaCwgYW5kIElEAIEcBwCBAw8AiDENAIEMBwCBRQZieQCLdQUgSUQAhCMbdHVybgCEXQwgdG8gYwCMOwUKCgCLL2oAjXUMAIwTDwCPNQotPisAjhwQOgCORQdlcgCMVwYAg3YIZWJob29rIFVSTCwgU2NvcGUAkAEGSUQAjwoOAI5rDSAAi2UKAINFBQCLYw0AHBEAgzUOOiBuAIE2DABgCACDCB1oZQCBaQ5JRACDYwUAahIAghB4RmxvdwCJMwkAjE0IAIV0CmhhbmdlAJIcBQCEYQVkYXRhIC0gdHlwaWNhbCB2aWEAkjQFcmFjdGluZwCSNQYAjV8LdmlhAJEhBwoAkVwKIACNfhAAhAsNAJJ5BQCCWQ8AhhYFAIVQFisATQsAimEKYwCBMgUAik8NAIhvB2FjaACHKAwAiAkFYW5kICJzaW5jZSIAiBsGAJNKDgCIAQoAhB0cAIFSCwCHWhwAgjwFYW5kIG5ldwBhFACHXxM6IFVwZGF0ZSBzdGF0dQCBKQYAgUMFADETCgoK&s=mscgen
