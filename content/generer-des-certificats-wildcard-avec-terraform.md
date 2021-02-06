+++
categories = []
date = 2021-02-01T23:00:00Z
description = ""
draft = true
image = ""
tags = []
title = "Générer des certificats wildcard avec Terraform"
type = "post"

+++
![Terraform + Let's Encrypt](https://blog.ledez.net/images/2019/10/terraform_plus_letsencrypt.png)

Vous connaissez [Terraform](https://www.terraform.io/) ? Et [Let’s Encrypt](https://letsencrypt.org/) ?

Le premier permet de gérer le cycle de vie de vos infrastructures. Et le deuxième vous permet d’avoir des certificats TLS signés par une autorité de certification valide dans les navigateurs modernes.

Dans cet article, je vais vous montrer comment faire un certificat wildcard `*.mondomaine.tld` avec Terraform. La validation avec le protocole acme la plus simple pour ces certificats, est via le DNS. Pour cela, je vais utiliser OVH puisque mon domaine de test est là-bas.

Bon, ce n’est pas le tout. Mais on n’est pas là pour beurrer des toasts.

Déjà, on va récupérer le repository qui permet de faire tout ça :

    $ git clone https://github.com/nledez/hashicorppourlesnoobs-sources.git demo-terraform-le
    $ cd demo-terraform-le/terraform
    $ ls
    install.sh                 ovh-create-consumer-key.py terraform.tf               variables.tf
    lecertificate.tf           requirements.txt           terraform.tfvars

À quoi servent ces fichiers ?

* `install.sh` -> installer les dépendances
* `requirements.txt` -> les dépendances Python
* `ovh-create-consumer-key.py` -> un script pour simplifier la création du fichier ovh.conf et ovh.sh
* `terraform.tf` -> fichier Terraform qui définit la configuration des providers Terraform
* `variables.tf` -> fichier Terraform qui définit les variables que l’on va utiliser dans notre plan Terraform
* `terraform.tfvars` -> fichier Terraform qui donne les valeurs que l’on va utiliser pour générer notre certificat TLS
* `lecertificate.tf` -> le plan Terraform qui permet de générer le certificat TLS chez Let’s Encrypt

On va commencer par installer les prérequis pour causer avec l’API OVH et donc le DNS :