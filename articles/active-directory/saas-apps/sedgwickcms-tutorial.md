---
title: 'Självstudie: Azure Active Directory-integrering med Sedgwick CMS | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Sedgwick CMS.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.openlocfilehash: ccda5c050c9f8af711637ea45090b6647c6c3488
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674660"
---
# <a name="tutorial-azure-active-directory-integration-with-sedgwick-cms"></a>Självstudie: Azure Active Directory-integrering med Sedgwick CMS

I den här självstudien får du lära dig att integrera Sedgwick CMS med Azure Active Directory (Azure AD).
Att integrera Sedgwick CMS med Azure AD ger följande fördelar:

* Du kan styra i Azure AD som har åtkomst till Sedgwick CMS.
* Du kan göra det möjligt för användarna att logga in automatiskt till Sedgwick CMS (enkel inloggning) med deras Azure AD-konton.
* Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill ha mer information om SaaS-appintegrering med Azure AD läser du avsnittet om [programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Om du inte har en Azure-prenumeration kan du [skapa ett kostnadsfritt konto ](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Sedgwick CMS behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har en Azure AD-miljö kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/)
* Sedgwick CMS-aktiverad prenumeration för enkel inloggning

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du enkel inloggning med Azure AD i en testmiljö.

* Sedgwick CMS stöder **IDP** INITIERAd SSO

## <a name="adding-sedgwick-cms-from-the-gallery"></a>Lägga till Sedgwick CMS från galleriet

Om du vill konfigurera integreringen av Sedgwick CMS i Azure AD måste du lägga till Sedgwick CMS från galleriet i listan över hanterade SaaS-appar.

**Utför följande steg för att lägga till Sedgwick CMS från galleriet:**

1. I **[Azure-portalen](https://portal.azure.com)** går du till den vänstra navigeringspanelen och klickar på **Azure Active Directory** -ikonen.

    ![Azure Active Directory-knappen](common/select-azuread.png)

2. Gå till **Företagsprogram** och välj alternativet **Alla program** .

    ![Bladet Företagsprogram](common/enterprise-applications.png)

3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** högst upp i dialogrutan.

    ![Knappen Nytt program](common/add-new-app.png)

4. I rutan Sök skriver du **SEDGWICK CMS** , väljer **Sedgwick CMS** från resultat panelen och klickar sedan på **Lägg till** för att lägga till programmet.

    ![Sedgwick CMS i resultat listan](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet konfigurerar och testar du enkel inloggning i Azure AD med Sedgwick CMS baserat på en test användare som kallas **Britta Simon** .
För att enkel inloggning ska fungera måste en länk relation mellan en Azure AD-användare och den relaterade användaren i Sedgwick CMS upprättas.

Om du vill konfigurera och testa enkel inloggning med Sedgwick CMS i Azure AD måste du slutföra följande Bygg stenar:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
2. **[Konfigurera SEDGWICK CMS enkel inloggning-om](#configure-sedgwick-cms-single-sign-on)** du vill konfigurera enskilda Sign-On inställningar på program sidan.
3. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
4. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
5. **[Skapa SEDGWICK CMS test User](#create-sedgwick-cms-test-user)** – om du vill ha en motsvarighet till Britta Simon i Sedgwick CMS som är länkad till Azure AD-representation av användare.
6. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet aktiverar du enkel inloggning med Azure AD i Azure-portalen.

Utför följande steg för att konfigurera enkel inloggning med Sedgwick CMS i Azure AD:

1. I [Azure Portal](https://portal.azure.com/)på sidan **Sedgwick CMS** Application Integration väljer du **enkel inloggning** .

    ![Konfigurera länk för enkel inloggning](common/select-sso.png)

2. I dialogrutan **Välj en metod för enkel inloggning** väljer du läget **SAML/WS-Fed** för att aktivera enkel inloggning.

    ![Välja läge för enkel inloggning](common/select-saml-option.png)

3. På sidan **Konfigurera enkel inloggning med SAML** klickar du på **redigeringsikonen** för att öppna dialogrutan **Grundläggande SAML-konfiguration** .

    ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

4. På sidan **Konfigurera enkel inloggning med SAML** utför du följande steg:

    ![Information om enkel inloggning för Sedgwick CMS-domän och URL: er](common/idp-intiated.png)

    a. I textrutan **Identifierare** skriver du en URL med följande mönster:  

    - `expresspreview.sedgwickcms.net/voe/sso`
    - `claimlookup.com/Voe/sso`

    b. I textrutan **Svars-URL** skriver du in en URL med följande mönster:

    - `https://<subdomain>.sedgwickcms.net/voe/sso`
    - `https://claimlookup.com/Voe/sso`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifieraren och svars-URL. Kontakta [SEDGWICK CMS client support team](https://www.sedgwick.com/help) för att hämta dessa värden. Du kan även se mönstren som visas i avsnittet **Grundläggande SAML-konfiguration** i Azure-portalen.

5. På sidan **Set up Single Sign-On with SAML** (Konfigurera enkel inloggning med SAML) går du till avsnittet **SAML Signing Certificate** (SAML-signeringscertifikat), klickar på **Ladda ned** för att ladda ned **Federation Metadata-XML** från de angivna alternativen enligt dina behov och spara den på datorn.

    ![Länk för nedladdning av certifikatet](common/metadataxml.png)

6. I avsnittet **Konfigurera SEDGWICK CMS** kopierar du lämpliga URL: er enligt ditt krav.

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

    a. Inloggnings-URL

    b. Azure AD-identifierare

    c. Utloggnings-URL

### <a name="configure-sedgwick-cms-single-sign-on"></a>Konfigurera Sedgwick CMS Single Sign-On

Om du vill konfigurera enkel inloggning på **SEDGWICK CMS** -sidan måste du skicka den hämtade **XML-** filen med federationmetadata och lämpliga kopierade url: er från Azure Portal till [Sedgwick CMS support team](https://www.sedgwick.com/help). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare 

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

1. Gå till den vänstra rutan i Azure-portalen och välj **Azure Active Directory** , välj **Users** och sedan **Alla användare** .

    ![Länkarna ”Användare och grupper” och ”Alla grupper”](common/users.png)

2. Välj **ny användare** överst på skärmen.

    ![Knappen Ny användare](common/new-user.png)

3. Genomför följande steg i Användaregenskaper.

    ![Dialogrutan Användare](common/user-properties.png)

    a. I fältet **Namn** anger du **BrittaSimon** .
  
    b. I fältet **användar namn** `brittasimon@yourcompanydomain.extension` . Till exempel BrittaSimon@contoso.com

    c. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan Lösenord.

    d. Klicka på **Skapa** .

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet aktiverar du Britta Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till Sedgwick CMS.

1. I Azure Portal väljer du **företags program** , väljer **alla program** och väljer sedan **Sedgwick CMS** .

    ![Bladet Företagsprogram](common/enterprise-applications.png)

2. I listan program väljer du **SEDGWICK CMS** .

    ![Sedgwick CMS-länk i listan program](common/all-applications.png)

3. På menyn till vänster väljer du **Användare och grupper** .

    ![Länken ”Användare och grupper”](common/users-groups-blade.png)

4. Klicka på knappen **Lägg till användare** och välj sedan **Användare och grupper** i dialogrutan **Lägg till tilldelning** .

    ![Fönstret Lägg till tilldelning](common/add-assign-user.png)

5. I dialogrutan **Användare och grupper** väljer du **Britta Simon** i listan med användare och klickar på knappen **Välj** längst ned på skärmen.

6. Om du förväntar dig ett roll värde i SAML-kontrollen väljer du lämplig roll för användaren i listan i dialog rutan **Välj roll** och klickar sedan på knappen **Välj** längst ned på skärmen.

7. I dialogrutan **Lägg till tilldelning** klickar du på knappen **Tilldela** .

### <a name="create-sedgwick-cms-test-user"></a>Skapa Sedgwick CMS test User

I det här avsnittet skapar du en användare som heter Britta Simon i Sedgwick CMS. Arbeta med [SEDGWICK CMS support team](https://www.sedgwick.com/help) för att lägga till användare i Sedgwick CMS-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på panelen Sedgwick CMS på åtkomst panelen, bör du loggas in automatiskt på den Sedgwick-CMS som du ställer in SSO för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ytterligare resurser

- [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](./tutorial-list.md)

- [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Vad är villkorlig åtkomst i Azure Active Directory?](../conditional-access/overview.md)