---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 1f73c46d5bc259c94e58a48a8a6bdb883a4454fd
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369851"
---
주 `DemoApplication` 클래스 내에서 데이터베이스 스키마를 만들 새 Spring 빈을 구성합니다.

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

이 Spring 빈은 *schema.sql*이라는 파일을 사용하므로 *src/main/resources* 폴더에 해당 파일을 만듭니다.
