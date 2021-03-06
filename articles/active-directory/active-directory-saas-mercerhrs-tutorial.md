---
title: 'Didacticiel : Intégration de Azure Active Directory à Mercer BenefitsCentral (MBC) | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Mercer BenefitsCentral (MBC).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: c0a9fc6c906af96bd613c02e8b4248fc2125d9e1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34346126"
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Didacticiel : Intégration de Azure Active Directory à Mercer BenefitsCentral (MBC)

Dans ce didacticiel, vous allez apprendre à intégrer Mercer BenefitsCentral (MBC) à Azure Active Directory (Azure AD).

L’intégration de Mercer BenefitsCentral (MBC) à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Mercer BenefitsCentral (MBC).
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à Mercer BenefitsCentral (MBC) (par le biais de l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis


Pour configurer l’intégration de Mercer BenefitsCentral (MBC) à Azure AD, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Mercer BenefitsCentral (MBC) pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Mercer BenefitsCentral (MBC) à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Ajout de Mercer BenefitsCentral (MBC) à partir de la galerie
Pour configurer l’intégration de Mercer BenefitsCentral (MBC) dans Azure AD, vous devez ajouter Mercer BenefitsCentral (MBC) depuis la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Mercer BenefitsCentral (MBC) à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Bouton Azure Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![Panneau Applications d’entreprise][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application][3]

4. Dans la zone de recherche, tapez **Mercer BenefitsCentral (MBC)**, sélectionnez **Mercer BenefitsCentral (MBC)** à partir du volet de résultats puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Mercer BenefitsCentral (MBC) dans la liste des résultats](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Mercer BenefitsCentral (MBC) avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Mercer BenefitsCentral (MBC) équivalent dans Azure AD. En d’autres termes, une relation de lien entre un utilisateur Azure AD et un utilisateur Mercer BenefitsCentral (MBC) associé doit être établie.

Dans Mercer BenefitsCentral (MBC), affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **Username** pour établir la relation de lien.

Pour configurer et tester l’authentification unique Azure AD avec Mercer BenefitsCentral (MBC), vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Créer un utilisateur de test Mercer BenefitsCentral (MBC)](#create-a-mercer-benefitscentral-mbc-test-user)** pour avoir dans Mercer BenefitsCentral (MBC) un équivalent de Britta Simon lié à la représentation Azure AD associée.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Mercer BenefitsCentral (MBC).

**Pour configurer l’authentification unique Azure AD avec Mercer BenefitsCentral (MBC), procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **Mercer BenefitsCentral (MBC)**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Boîte de dialogue Authentification unique](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

3. Dans la section **Domaine et URL Mercer BenefitsCentral (MBC)**, procédez comme suit :

    ![Informations d’authentification unique dans Domaine et URL Mercer BenefitsCentral (MBC)](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `stg.mercerhrs.com/saml2.0`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > La valeur de l’URL de réponse n’est pas réelle. Mettez à jour la valeur avec l’URL de réponse réelle. Contactez [l’équipe de support Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) pour obtenir cette valeur.

4. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Lien Téléchargement de certificat](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

5. Cliquez sur le bouton **Enregistrer** .

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_400.png)

6. Dans la section **Configuration de Mercer BenefitsCentral (MBC)** , cliquez sur **Configurer Mercer BenefitsCentral (MBC)** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez l **’URL du service d’authentification unique SAML** à partir de la **section Référence rapide.**

    ![Configuration de Mercer BenefitsCentral (MBC)](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

7. Pour configurer l’authentification unique du côté **Mercer BenefitsCentral (MBC)**, vous devez envoyer le fichier **XML de métadonnées** et **l’URL du service d’authentification unique SAML** téléchargés à [l’équipe du support technique de Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Vous pouvez en savoir plus sur la fonctionnalité de documentation incorporée ici : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

   ![Créer un utilisateur de test Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet gauche du Portail Azure, cliquez sur le bouton **Azure Active Directory**.

    ![Bouton Azure Active Directory](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_01.png)

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_02.png)

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue **Tous les utilisateurs**.

    ![Bouton Ajouter](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_03.png)

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue Utilisateur](./media/active-directory-saas-mercerhrs-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail de l’utilisateur Britta Simon.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.

    d. Cliquez sur **Créer**.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Créer un utilisateur de test Mercer BenefitsCentral (MBC)

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Mercer BenefitsCentral (MBC). Travaillez avec [l’équipe de support Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) pour ajouter les utilisateurs dans la plateforme Mercer HRS. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Mercer BenefitsCentral (MBC).

![Attribuer le rôle utilisateur][200] 

**Pour affecter Britta Simon à Mercer BenefitsCentral (MBC), procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **Mercer BenefitsCentral (MBC)**.

    ![Lien Mercer BenefitsCentral (MBC) dans la liste des applications](./media/active-directory-saas-mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »][202]

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Volet Ajouter une attribution][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la mosaïque Mercer BenefitsCentral (MBC) dans le volet d’accès, vous devez être connecté automatiquement à votre application Mercer BenefitsCentral (MBC).
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mercerhrs-tutorial/tutorial_general_203.png

