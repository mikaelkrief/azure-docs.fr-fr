---
title: 'Didacticiel : Intégration d’Azure Active Directory à Directions on Microsoft | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Directions on Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1fb08ce46153fb9c8cc0b80736cc44093f829a02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34340792"
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Didacticiel : Intégration d’Azure Active Directory à Directions on Microsoft

Dans ce didacticiel, vous allez apprendre à intégrer Directions on Microsoft à Azure Active Directory (Azure AD).

L’intégration de Directions on Microsoft à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Directions on Microsoft.
- Vous pouvez autoriser les utilisateurs à se connecter automatiquement à Directions on Microsoft (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis


Pour configurer l’intégration d’Azure AD à Directions on Microsoft, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Abonnement Directions on Microsoft pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Directions on Microsoft à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-directions-on-microsoft-from-the-gallery"></a>Ajout de Directions on Microsoft à partir de la galerie
Pour configurer l’intégration de Directions on Microsoft à Azure AD, vous devez ajouter Directions on Microsoft disponible dans la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter Directions on Microsoft à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

4. Dans la zone de recherche, tapez **Directions on Microsoft**.

    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. Dans le volet de résultats, sélectionnez **Directions on Microsoft**, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Directions on Microsoft, avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Directions on Microsoft équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et l’utilisateur Directions on Microsoft associé doit être établie.

Dans Directions on Microsoft, affectez la valeur du **nom d’utilisateur** dans Azure AD comme valeur du **nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec Directions on Microsoft, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur de test Directions on Microsoft](#creating-a-directions-on-microsoft-test-user)** pour avoir un équivalent de Britta Simon dans Directions on Microsoft lié à la représentation Azure AD de l’utilisateur.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Directions on Microsoft.

**Pour configurer l’authentification unique Azure AD avec Directions on Microsoft, procédez comme suit :**

1. Dans le portail Azure, dans la page d’intégration de l’application **Directions on Microsoft**, cliquez sur **Authentification unique**.

    ![Configure Single Sign-On][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. Dans la section **Domaine et URL Directions on Microsoft**, procédez comme suit :

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant :
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    b. Dans la zone de texte **Identificateur**, entrez une URL au format suivant :
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez [l’équipe du support client Directions on Microsoft](mailto:service@DirectionsOnMicrosoft.com). 
 
4. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. Cliquez sur le bouton **Enregistrer** .

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. Pour configurer l’authentification unique côté **Directions on Microsoft**, vous devez envoyer les **métadonnées XML** téléchargées à [l’équipe du support technique Directions on Microsoft](mailto:service@DirectionsOnMicrosoft.com). Pour permettre à l’équipe du support technique Directions on Microsoft de trouver votre adhésion au site fédéré, indiquez les informations de votre entreprise dans votre e-mail.
    
    >[!NOTE]
    >L’authentification unique pour Directions on Microsoft doit être activée par [l’équipe du support client Directions on Microsoft](mailto:service@DirectionsOnMicrosoft.com). Vous recevrez une notification une fois l’authentification unique activée.

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Vous pouvez en savoir plus sur la fonctionnalité de documentation incorporée ici : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-a-directions-on-microsoft-test-user"></a>Création d’un utilisateur de test Directions on Microsoft

Aucun élément d’action ne vous permet de configurer l’approvisionnement des utilisateurs dans Directions on Microsoft.  

Quand un utilisateur affecté tente de se connecter à Directions on Microsoft à l’aide du volet d’accès, Directions on Microsoft vérifie l’existence de l’utilisateur. Si aucun compte d’utilisateur n’est disponible, Directions on Microsoft le crée automatiquement.

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Directions on Microsoft.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à Directions on Microsoft, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **Directions on Microsoft**.

    ![Configure Single Sign-On](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.
 
Lorsque vous cliquez sur la vignette Directions on Microsoft dans le volet d’accès, vous devez être connecté automatiquement à votre application Directions on Microsoft.

Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

