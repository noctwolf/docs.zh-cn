---
title: 实体框架概述
ms.date: 09/17/2018
ms.assetid: a2166b3d-d8ba-4a0a-8552-6ba1e3eaaee0
ms.openlocfilehash: 498d0478dc94048a69c393151d3ff8a752706e1c
ms.sourcegitcommit: b5c59eaaf8bf48ef3ec259f228cb328d6d4c0ceb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67539467"
---
# <a name="entity-framework-overview"></a>实体框架概述

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]是 ADO.NET 中的一套支持开发面向数据的软件应用程序的技术。 面向数据的应用程序的架构师和开发人员曾为实现两个迥然不同的目标费尽心机： 他们必须为要解决的业务问题的实体、关系和逻辑构建模型，还必须处理用于存储和检索数据的数据引擎。 数据可能跨多个各有不同协议的存储系统；甚至使用单个存储系统的应用程序也必须在存储系统的需求与编写高效且容易维护的应用程序代码之间取得平衡。

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] 使开发人员可以采用特定于域的对象和属性（如客户和客户地址）的形式使用数据，而不必自己考虑存储这些数据的基础数据库表和列。 借助[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]，开发人员在处理数据时能够以更高的抽象级别工作，并且能够以相比传统应用程序更少的代码创建和维护面向数据的应用程序。 因为[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]是.NET Framework 的组件[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]应用程序可以在其安装.NET Framework 版本 3.5 SP1 的任何计算机上运行。

## <a name="give-life-to-models"></a>赋予模型生命
 构建应用程序或服务时常用的一种长期存在的设计方法是将应用程序或服务分为三部分：域模型、逻辑模型和物理模型。 域模型定义要建模的系统中的实体和关系。 关系数据库的逻辑模型通过外键约束将实体和关系规范化到表中。 物理模型通过指定分区和索引等存储详细信息实现特定数据引擎的功能。

 物理模型由数据库管理员进行优化以改善性能，而编写应用程序代码的程序员的工作主要限制为通过编写 SQL 查询和调用存储过程来处理逻辑模型。 域模型通常用作捕获和沟通应用程序需求的工具，常常以静态关系图形式提供，用于在项目早期阶段查看和讨论之用，随后会被弃用。 许多开发团队会跳过概念模型的创建，直接从指定关系数据库中的表、列和键开始工作。

 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]开发人员能够在域模型中查询实体和关系，从而赋予模型生命 (称为*概念*中的模型[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]) 时依赖于[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]转换这些对数据源特定于命令的操作。 这使应用程序不再对特定数据源具有硬编码的依赖性。

 在使用 Code First 时，概念模型在代码中映射到存储模型。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]可以基于对象类型和您定义的其他配置推断概念模型。 基于您定义域类型的方式和在代码中提供的其他配置信息的组合，在运行时生成映射元数据。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]基于元数据根据需要生成数据库。 有关详细信息，请参阅[创建和映射概念模型](https://go.microsoft.com/fwlink/?LinkID=235382)。

 使用实体数据模型工具时，概念模型、存储模型以及这两者之间的映射以基于 XML 的架构表示，并在具有对应扩展名的文件中定义：

- 概念架构定义语言 (CSDL) 定义概念模型。 CSDL 是[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]的实现[实体数据模型](../../../../../docs/framework/data/adonet/entity-data-model.md)。 文件扩展名为 .csdl。

- 存储架构定义语言 (SSDL) 定义存储模型，也称为“逻辑模型”。 文件扩展名为 .ssdl。

- 映射规范语言 (MSL) 定义存储模型与概念模型之间的映射。 文件扩展名为 .msl。

可以根据需要对存储模型和映射进行更改，而无需对概念模型、数据类或应用程序代码进行更改。 存储模型是特定于提供程序的，因此可以在各种数据源之间使用一致的概念模型。

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]使用这些模型和映射文件来创建、 读取、 更新和删除操作中的数据源中的等效操作对概念模型实体和关系。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]甚至支持到数据源中的存储过程在概念模型中的实体映射。 有关详细信息，请参阅[CSDL、 SSDL 和 MSL 规范](../../../../../docs/framework/data/adonet/ef/language-reference/csdl-ssdl-and-msl-specifications.md)。

## <a name="map-objects-to-data"></a>对数据的 map 对象
 面向对象的编程对与数据存储系统的交互提出了一个难题。 虽然类的组织结构通常可以比较接近地反映关系数据库表的组织结构，但这种对应关系并不完美。 多个规范化表通常对应于单个类，而且类间关系的表示方式与表间关系的表示方式通常也不相同。 例如，若要表示某个销售订单的客户，`Order` 类可能会使用一个包含对 `Customer` 类实例的引用的属性，而数据库中的 `Order` 表行会包含一个外键列（或一组列），通过这些列包含对应于 `Customer` 表中的主键值的值。 `Customer` 类可能会使用一个名为 `Orders` 的属性（该属性包含 `Order` 类实例的集合），而数据库中的 `Customer` 表则不包含相应的列。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]让开发人员可以灵活地采用此方式表示关系，或更贴切地对在数据库中表示的关系进行建模。

 现有解决方案只能通过将面向对象的类和属性映射到关系表和列来尝试弥合这种通常称为“阻抗不匹配”的差异。 而不会采用这种传统方法，[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]映射到概念模型中实体和关系的关系表、 列和逻辑模型中的外键约束。 这在定义对象和优化逻辑模型方面都增加了灵活性。 Entity Data Model 工具生成基于概念模型的可扩展的数据类。 这些类是分部类，可以通过开发人员添加的其他成员进行扩展。 默认情况下，为特定概念模型生成的类派生自基类，这些基类提供服务以将实体具体化为对象以及跟踪和保存更改。 开发人员可以使用这些类像处理通过关联相关的对象一样处理实体和关系。 开发人员还可以自定义针对某一概念模型生成的类。 有关详细信息，请参阅[使用对象](../../../../../docs/framework/data/adonet/ef/working-with-objects.md)。

## <a name="access-and-change-entity-data"></a>访问和更改实体数据

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]不仅仅是另一种对象关系映射解决方案，从本质上讲，它的作用是使应用程序能够访问和更改概念模型中以实体和关系形式表示的数据。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]使用模型和映射文件中的信息将对概念模型中表示的实体类型的对象查询转换为特定于数据源的查询。 查询结果具体化为对象的[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]管理。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]提供以下方式来查询概念模型并返回对象：

- LINQ to Entities。 提供语言集成查询 (LINQ) 支持，用于查询在概念模型中定义的实体类型。 有关详细信息，请参阅[LINQ to Entities](../../../../../docs/framework/data/adonet/ef/language-reference/linq-to-entities.md)。

- [!INCLUDE[esql](../../../../../includes/esql-md.md)]。 与存储无关的 SQL 方言直接使用概念模型中的实体并支持实体数据模型概念。 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 可用于对象查询和使用 EntityClient 提供程序执行的查询。 有关详细信息，请参阅[实体 SQL 概述](../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)。

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] 中包含 EntityClient 数据提供程序。 此提供程序管理连接，将实体查询转换为特定于数据源的查询，并返回[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]用于将实体数据具体化为对象的数据读取器。 当不需要对象具体化时，通过使应用程序执行 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询并使用返回的只读数据读取器，还可以像标准 ADO.NET 数据提供程序一样使用 EntityClient 提供程序。 有关详细信息，请参阅[针对实体框架的 EntityClient Provider](../../../../../docs/framework/data/adonet/ef/entityclient-provider-for-the-entity-framework.md)。

下图阐释了用于访问数据的[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]体系结构：

![实体框架体系结构图](../../../../../docs/framework/data/adonet/ef/media/wd-efarchdiagram.gif "wd_EFArchDiagram")

实体数据模型工具可以生成一个类派生自`System.Data.Objects.ObjectContext`或`System.Data.Entity.DbContext`该类表示概念模型中的实体容器。 此对象上下文提供跟踪更改以及管理标识、并发和关系的功能。 此类还公开将插入、更新和删除操作写入数据源的 `SaveChanges` 方法。 与查询类似，这些更改是由系统自动生成的命令或由开发人员指定的存储过程执行的。

## <a name="data-providers"></a>数据提供程序

`EntityClient`提供程序通过访问概念实体和关系方面的数据来扩展 ADO.NET 提供程序模型。 它执行使用 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 的查询。 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 提供使 `EntityClient` 能与数据库进行通信的基础查询语言。 有关详细信息，请参阅[针对实体框架的 EntityClient Provider](../../../../../docs/framework/data/adonet/ef/entityclient-provider-for-the-entity-framework.md)。

[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] 包含一个支持规范命令目录树的最新 SqlClient 数据提供程序。 有关详细信息，请参阅[用于实体框架的 SqlClient](../../../../../docs/framework/data/adonet/ef/sqlclient-for-the-entity-framework.md)。

## <a name="entity-data-model-tools"></a>实体数据模型工具

连同[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]运行时，Visual Studio 包括映射和建模工具。 有关详细信息，请参阅[建模和映射](../../../../../docs/framework/data/adonet/ef/modeling-and-mapping.md)。

## <a name="learn-more"></a>了解详细信息

若要详细了解[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]，请参阅：

[入门教程](../../../../../docs/framework/data/adonet/ef/getting-started.md)-介绍如何启动和运行使用快速[快速入门教程](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399182(v=vs.100))，其中说明了如何创建一个简单[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]应用程序。

[实体框架术语](../../../../../docs/framework/data/adonet/ef/terminology.md)-定义的许多术语所引入的实体数据模型和[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]中使用和[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]文档。

[实体框架资源](../../../../../docs/framework/data/adonet/ef/resources.md)-提供概念性主题的链接和链接到外部主题和资源来构建[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]应用程序。

## <a name="see-also"></a>请参阅

- [ADO.NET 实体框架](../../../../../docs/framework/data/adonet/ef/index.md)
