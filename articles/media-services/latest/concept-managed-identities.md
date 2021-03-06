---
title: Felügyelt identitások
description: A Media Services az Azure által felügyelt identitásokkal használható.
keywords: ''
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 1/29/2020
ms.author: inhenkel
ms.openlocfilehash: 0bbfb54d6ba7483e96633bdf05bb580e5517d216
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/03/2021
ms.locfileid: "106277742"
---
# <a name="managed-identities"></a>Felügyelt identitások

A fejlesztők számára gyakori kihívás a titkok és a hitelesítő adatok kezelése a különböző szolgáltatások közötti kommunikáció biztonságossá tételéhez. Az Azure-ban a felügyelt identitások nem teszik lehetővé, hogy a fejlesztők a hitelesítő adatok kezeléséhez a személyazonosságot az Azure AD-ben, az Azure AD-ben pedig Azure Active Directory (Azure AD-) tokenek beszerzéséhez használják.

Jelenleg két forgatókönyv használható a felügyelt identitások használatára a Media Services használatával:

- A Storage-fiókok eléréséhez használja a Media Services fiók felügyelt identitását.

- A Media Services fiók felügyelt identitásával férhet hozzá az ügyfelek kulcsaihoz Key Vaulthoz.

A következő két szakasz ismerteti a két forgatókönyv lépéseit.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>A Media Services fiók felügyelt identitásának használata a Storage-fiókok eléréséhez

1. Hozzon létre egy Media Services fiókot felügyelt identitással.
1. Adja meg a felügyelt identitás elsődleges hozzáférését egy Ön által birtokolt Storage-fiókhoz.
1. Media Services ezután a felügyelt identitás használatával férhet hozzá a Storage-fiókhoz az Ön nevében.

## <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>A Media Services fiók felügyelt identitásával férhet hozzá az ügyfelek kulcsaihoz Key Vaulthoz

1. Hozzon létre egy Media Services fiókot felügyelt identitással.
1. Adja meg a felügyelt identitás elsődleges hozzáférését egy Ön által birtokolt Key Vaulthoz.
1. Konfigurálja a Media Services fiókot az ügyfél-kulcson alapuló fiók titkosításának használatára.
1. A Media Services a felügyelt identitás használatával fér hozzá a Key Vaulthoz az Ön nevében.

További információ az ügyfél által felügyelt kulcsokról és Key Vaultekről: [saját kulcs használata (ügyfél által felügyelt kulcsok) a Media Services](concept-use-customer-managed-keys-byok.md)

## <a name="tutorials"></a>Oktatóanyagok

Ezek az oktatóanyagok a fent említett forgatókönyveket is tartalmazzák.

- [Az ügyfél által felügyelt kulcsok vagy BYOK használata a Azure Portal használatával Media Services](security-customer-managed-keys-portal-tutorial.md)
- [Az ügyfél által felügyelt kulcsokat vagy BYOK Media Services REST API használhatja](security-customer-managed-keys-rest-postman-tutorial.md).

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne megtudni arról, hogy az Ön és az Azure-alkalmazások milyen felügyelt identitásokkal rendelkeznek, tekintse meg az [Azure ad által felügyelt identitások](../../active-directory/managed-identities-azure-resources/overview.md)című témakört.
