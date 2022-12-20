# Article Sur Les Architecture Components

Les Architecture Components sont un ensemble de bibliothèques conçues par Google pour aider les développeurs d'applications Android à construire des applications robustes, fiables et maintenables. Ces bibliothèques couvrent différents aspects de l'architecture d'une application, tels que la gestion de l'état, la communication entre les composants de l'application, la persistance des données et la prise en charge de l'interface utilisateur.

### Voici quelques-unes des principales bibliothèques incluses dans les Architecture Components :

* **LiveData** : une classe qui permet de diffuser des données à travers l'application de manière observée. Les objets LiveData sont liés aux données qu'ils contiennent, et ils notifient les observateurs lorsque ces données changent. Cela permet aux parties de l'application de mettre à jour leur interface utilisateur de manière réactive lorsque les données sous-jacentes changent.

* **ViewModel** : une classe qui stocke et gère l'état des données associées à une interface utilisateur. Les ViewModel sont conçus pour être liés à une activité ou un fragment, mais ils sont indépendants de l'interface utilisateur elle-même. Cela signifie qu'ils ne sont pas détruits lorsque l'interface utilisateur est reconstruite, ce qui est utile lorsque l'application est en rotation de téléphone ou lorsque l'utilisateur change d'orientation.

* **Room** : une bibliothèque de persistance de données qui facilite la gestion des données en cache dans une application Android. Room gère la création et la mise à jour de la base de données, ainsi que les requêtes de lecture et d'écriture. Il offre également une couche de mappage objet-relationnel (ORM) pour convertir les objets Java en enregistrements de base de données et inversement.

* **WorkManager** : une bibliothèque qui permet aux applications de planifier et de gérer des tâches de manière fiable même lorsque l'application n'est pas en cours d'exécution. WorkManager prend en charge différents types de tâches, y compris des tâches périodiques, des tâches qui doivent être exécutées une seule fois lorsque l'application est prochaine du démarrage et des tâches qui doivent être exécutées lorsque l'appareil est connecté à un réseau de données.

En utilisant les Architecture Components, les développeurs peuvent créer des applications Android robustes et faciles à maintenir en suivant les meilleures pratiques recommandées par Google.

### Voici quelques exemples pour chacun des points ci-dessus : 

#### Voici comment utiliser LiveData avec Kotlin :

1. Créez une instance de LiveData en définissant le type de données qu'elle contiendra :

`val liveData = MutableLiveData<String>()`

2. Définissez la valeur initiale de LiveData si nécessaire :

`liveData.value = "Initial value"`

3. Modifiez la valeur de LiveData à tout moment en utilisant la méthode `setValue()` ou `postValue()` :

`liveData.value = "New value"`  
`liveData.postValue("New value")`

La méthode `setValue()` met à jour la valeur de LiveData de manière synchrone, tandis que `postValue()` le fait de manière asynchrone. Utilisez `postValue()` si vous souhaitez mettre à jour la valeur de LiveData depuis un thread différent du thread principal.

4. Observez les changements de valeur de LiveData à l'aide de la méthode `observe()` :

`liveData.observe(this, Observer { value ->
// code à exécuter lorsque la valeur de LiveData change
})
`

Vous pouvez également utiliser la méthode `observeForever()` pour observer les changements de valeur de LiveData indéfiniment, même lorsque l'activité ou le fragment n'est plus en fonctionnement. N'oubliez pas de désabonner l'observer en utilisant la méthode `removeObserver()` lorsque vous n'en avez plus besoin.

En résumé, LiveData est un objet de données observable utile pour transmettre des données entre différentes parties de votre application Android de manière efficace et sans devoir gérer manuellement les mises à jour de l'interface utilisateur. Utilisez-le en conjonction avec le modèle de conception Android Architecture Components pour créer des applications robuste.

#### Voici comment utiliser une ViewModel avec Kotlin :

1. Créez une classe héritant de ViewModel et définissez les données que vous souhaitez stocker dans la ViewModel. Par exemple :

`class MainViewModel : ViewModel() {
// Données à stocker dans la ViewModel
val userName: MutableLiveData<String> = MutableLiveData()
val userEmail: MutableLiveData<String> = MutableLiveData()
}`

2. Dans votre activité ou votre fragment, obtenez une instance de la ViewModel en appelant la méthode `ViewModelProviders.of(this).get(MainViewModel::class.java)`. Cette méthode prend en paramètre l'activité ou le fragment courant et retourne une instance de la ViewModel.

3. Utilisez les données de la ViewModel en y accédant directement, comme ceci :

`val mainViewModel = ViewModelProviders.of(this).get(MainViewModel::class.java)`  
`mainViewModel.userName.value = "John Doe"`  
`mainViewModel.userEmail.value = "john.doe@example.com"`

4. Pour afficher les données de la ViewModel dans votre interface utilisateur, utilisez un `Observer` pour surveiller les changements de valeur de la ViewModel. Par exemple :

`mainViewModel.userName.observe(this, Observer { userName ->
// Mettre à jour l'interface utilisateur avec la nouvelle valeur de userName
})
`

Il est important de noter que les ViewModels sont conçues pour être utilisées avec des données en lecture seule et ne sont pas conçues pour être utilisées comme couche de persistance de données. Pour stocker des données de manière persistante, vous devriez utiliser une autre solution telle que la base de données Room.

#### Voici comment utiliser Room avec Kotlin :

1. Ajoutez la dépendance Room à votre fichier `build.gradle` :

`implementation "androidx.room:room-runtime:2.2.5"`

2. Créez une classe annotée avec @Entity qui représente une table dans votre base de données. Définissez les colonnes de la table en utilisant les annotations `@ColumnInfo` et `@PrimaryKey`. Par exemple :

`@Entity
data class User(
@PrimaryKey val id: Long,
@ColumnInfo(name = "name") val name: String,
@ColumnInfo(name = "email") val email: String
)
`

3. Créez une interface annotée avec `@Dao` qui définit les méthodes pour accéder à la table. Par exemple :

`@Dao
interface UserDao {
@Query("SELECT * FROM user")
fun getAll(): List<User>`

    @Insert
    fun insert(user: User)

    @Update
    fun update(user: User)

    @Delete
    fun delete(user: User)
`}`

4. Créez une classe annotée avec `@Database` qui étend de `RoomDatabase` et inclut les DAOs de votre application. Par exemple :

`@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
abstract fun userDao(): UserDao
}
`

5. Créez une instance de la base de données en appelant la méthode `Room.databaseBuilder()`. Vous pouvez utiliser cette instance pour accéder aux DAOs et exécuter des requêtes sur la base de données. Par exemple :

`val db = Room.databaseBuilder(
context.applicationContext,
AppDatabase::class.java, "database-name"
).build()`

`val userDao = db.userDao()`
`val users = userDao.getAll()`

Il est important de noter que Room utilise les annotations pour générer du code à l'aide de l'outil de génération de code de Android (Android Annotations Processor). Vous devrez ajouter cet outil à votre fichier `build.gradle` pour que Room fonctionne correctement :

`kapt "androidx.room:room-compiler:2.2.5"`

#### Voici comment utiliser WorkManager avec Kotlin :

1. Ajoutez la dépendance WorkManager à votre fichier `build.gradle` :

`implementation "androidx.work:work-runtime-ktx:2.4.0"`

2. Créez une classe héritant de `Worker` et implémentez la méthode `doWork()`, qui définit le travail à effectuer. Vous pouvez utiliser les méthodes `inputData` et `outputData` pour passer des arguments et des résultats à la tâche. Par exemple :

`class MyWorker(context: Context, params: WorkerParameters) : Worker(context, params) {
override fun doWork(): Result {
val input = inputData.getString("input_key")
// Effectuer le travail ici
val output = "result"
outputData = workDataOf("output_key" to output)
return Result.success()
}
}
`

3. Créez une instance de `WorkRequest` en utilisant la classe `OneTimeWorkRequestBuilder` et définissez les paramètres de la tâche, comme ceci :

`val work = OneTimeWorkRequestBuilder<MyWorker>()
.setInputData(workDataOf("input_key" to "input"))
.build()
`

4. Enfin, utilisez l'instance de `WorkManager` pour planifier la tâche en appelant la méthode `enqueue()` :

`WorkManager.getInstance(context).enqueue(work)`

Vous pouvez également utiliser `PeriodicWorkRequest` pour planifier des tâches périodiques au lieu de tâches à exécuter une seule fois. Vous pouvez également utiliser des `Constraints` pour définir des conditions qui doivent être remplies avant que la tâche ne soit exécutée (par exemple, lorsque le réseau est disponible).

Il est important de noter que WorkManager utilise des `Worker` pour exécuter les tâches et qu'il gère automatiquement la reprise des tâches en cas d'échec ou de redémarrage de l'application. Cela signifie que vous n'avez pas à vous inquiéter de la gestion des tâches en arrière-plan dans votre application.

