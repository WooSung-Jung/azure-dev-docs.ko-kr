---
title: 이미지 나열
description: 가상 머신을 만드는 데 사용할 수 있는 모든 이미지를 인쇄합니다.
ms.topic: conceptual
ms.date: 6/15/2017
ms.openlocfilehash: 5ce7525ffe87af544c6b8bb4b74dc9a3a0c035cb
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441668"
---
# <a name="list-images"></a>이미지 나열

다음 코드를 사용하여 모든 SKU 및 버전을 포함하여 가상 머신을 만드는 데 사용할 수 있는 모든 이미지를 인쇄합니다.

```python
region = 'eastus'

result_list_pub = compute_client.virtual_machine_images.list_publishers(
    region,
)

for publisher in result_list_pub:
    result_list_offers = compute_client.virtual_machine_images.list_offers(
        region,
        publisher.name,
    )

    for offer in result_list_offers:
        result_list_skus = compute_client.virtual_machine_images.list_skus(
            region,
            publisher.name,
            offer.name,
        )

        for sku in result_list_skus:
            result_list = compute_client.virtual_machine_images.list(
                region,
                publisher.name,
                offer.name,
                sku.name,
            )

            for version in result_list:
                result_get = compute_client.virtual_machine_images.get(
                    region,
                    publisher.name,
                    offer.name,
                    sku.name,
                    version.name,
                )

                print('PUBLISHER: {0}, OFFER: {1}, SKU: {2}, VERSION: {3}'.format(
                    publisher.name,
                    offer.name,
                    sku.name,
                    version.name,
                ))
```
