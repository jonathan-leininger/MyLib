# MyLib

Comment créer sa librairie sous forme de Pod.


## Pour info, 
Les pods disponibles publiquement sont tous recensés sur le master repo de CocoaPods nommé `Specs` ici : https://github.com/CocoaPods/Specs 
Lorsqu’on fait un `pod update` on vient mettre à jour notre repo local. Ça revient en gros à faire un `git pull` sur la master du repo de CocoaPods. 
Du coup quand on fait un `pod install toto`, on regarde si le pod `toto` est sur l’image du repo `Specs` de CocoaPods qu'on a en local sur notre machine. C’est pour ça que dès fois il nécessaire de rafraîchir notre repo local par un pod update.


## Créer un pod pour qui ?
Lorsqu’on créé un pod, 

### On peut souhaiter le rendre public, 
* Sur le repo Specs de CocoaPods 
* Sur notre repo “Specs” perso public (sans grand intérêt)
* Sans passer par un repo Specs (mais juste donner un accès au git)

### On peut souhaiter le rendre privé,
* Sur un Repo Specs privé
* Sans passer par un repo Specs


## Exemple :
1) Création de 3 repos `Specs`, `MyLib`, `MyApp`
2) Ajouter votre repo privé à pod sur votre machine : 
```ruby
pod repo add MyPods https://github.com/jonathan-leininger/Specs.git
``````
3) Vérifier que le repo est bien ajouté : `ls ~/.cocoapods/repos`
4) Initialiser un projet Xcode de type pod : 
```ruby
pod lib create MyLib
``````
5) Ouvrir `MyLib.xcworkspace`
6) Editer le fichier `MyLib.podspec` : renseigner les informations demandées
7) A la place du fichier `replaceMe.swift` ajouter le code source de votre librairie. Toutes les classes et fonctions visibles doivent être préfixées de `public`
8) Valider que le projet est bien configuré : 
```ruby
pod lib lint MyLib.podspec --allow-warnings
``````
9) Pusher tout ça sur la branche `master`
10) Ajouter la lib à votre repo : 
```ruby
pod repo push MyPods MyLib.podspec --allow-warnings
``````
11) Créer votre projet`MyApp`, puis dans votre podfile ajouter :
```ruby
source 'https://github.com/jonathan-leininger/Specs.git'
source 'https://github.com/CocoaPods/Specs.git'
pod "MyLib"
``````
10) pod install
On peut passer outre le fait d'avoir un repo privé en ajoutant l'URL du Git : 
```ruby
pod "MyLib", :git => "https://github.com/jonathan-leininger/MyLib.git", :tag => '0.9.3'
``````
Sur le repo public de Cocoa pod, il vous faudra faire ainsi :
```ruby
pod trunk register jonathan.leininger@open-groupe.com 'Jonathan Leininger'
pod trunk push MyLib.podspec --allow-warnings
``````
MàJ votre repo local : `pod repo update`


## Tutos :
Il existe plusieurs tutos sur le net, mais je n'ai pas réussi à en trouver une qui expliquait tout et qui marchait de bout en bout !
https://code.tutsplus.com/tutorials/creating-your-first-cocoapod--cms-24332
https://code.tutsplus.com/tutorials/managing-private-pods-with-cocoapods--cms-25137
https://www.raywenderlich.com/126365/ios-frameworks-tutorial
https://medium.com/@shahabejaz/create-and-distribute-private-libraries-with-cocoapods-5b6507b57a03
https://guides.cocoapods.org/making/private-cocoapods.html
https://guides.cocoapods.org/making/making-a-cocoapod.html
https://guides.cocoapods.org/making/using-pod-lib-create.html
