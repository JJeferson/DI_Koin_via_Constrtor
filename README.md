# DI_Koin_via_Constrtor
                                          Implementando Injeção de Dependências com Koin
                                                                                                                
                                          Alguns lembretes antes de começar:
                                          1)Isso é para Android Nativo.
                                          2)O design pattern de injeção de dependencias no kotlin, com Koin, é usado
                                          basicamente para tornar mais simples e seguro o manuseio de tusa viewmodel.
                                                            
                                          1)Você começa acessando o site do Koin:
                                          https://insert-koin.io/
                                                            
                                          2)Dentro do Gradle você declara as dependencias.
                                          Lembre-se de conferir no site do Koin a ultima versão da dependencia.
                                          Fica:
                                                            
                                         // Koin for Kotlin
                                         implementation  'org.koin:koin-core:2.2.0'
                                         // Koin for Android
                                         implementation  'org.koin:koin-android:2.2.0'
                                         // or Koin for Lifecycle scoping
                                         implementation  'org.koin:koin-androidx-scope:2.2.0'
                                         // or Koin for Android Architecture ViewModel
                                         implementation  'org.koin:koin-androidx-viewmodel:2.2.0'
                                         // or Koin for Android Fragment Factory (unstable version)
                                         implementation  'org.koin:koin-androidx-fragment:2.2.0'
                                                            
                                         3)Crie um package para armazenar as classes e arquivos referentes 
                                         ao Koin.
                                         Normalmente vc vai chama-lo de DI(dependency injection)
                                                            
                                         4)Crie um novo arquivo kotlin:
                                         Lembrete se você tiver mais de um viewmodel, cada um para uma função
                                         diferente pode ser adequado criar novo package com nome da sua viewmodel
                                         e lá criar os arquivos do koin referentes a ele. 
                                         exemplo: DI\Eventos\
                                                            ModuloEVENTOS.kt
                                                             AplicationEventos.kt
                                        O primeiro arquivo que vc vai criar é o Modulo. De nome como exemplificado a cima.
                                        ModuloPRAQUEELESERVE.kt
                                        Ali dentro:
                                        val ModuloPRAQUEELESERVE = module{
                                         single { AQUI_SEU_REPOSITORO_repository() }

                                         viewModel {  AQUI_SUA_viewmodel ( AQUI_SEU_REPOSITORO_repository= get() ) } }
                                       Vai ficar + ou - assim:
                                                                   
                                       val ModuloEventos = module{
                                                           single { repositoryEventos() }
                                                           viewModel {  viewmodelEventos ( repositoryEventos= get() ) } }
                                                                   
                                       Ele vai apresentar erro por causa do construtor da sua Viewmodel referenciada.
                                       Dai você deve adequa-la no construtor. 
                                       Tome cuidado com esta parte, eventualmente tu pode ter
                                       de refatorar parte do teu fonte dentro dela pra não dar erros.
                                       Exemplo:
                                       viewmodelEventos(val repositoryEventos :repository): ViewModel(), LifecycleOwner  {
                                                           
                                      5)
                                      Arquivo de aplicação:
                                      Dentro do package crie uma classe do kotlin para ser a classe de  aplicação pro Koin:
                                                           
                                      class AplicationKoin: Application() {

                                      override fun onCreate(){
                                      super.onCreate()

                                      startKoin{
                                      androidContext(this@AplicationKoin)
                                      modules(ModuloEventos)
                                       } } }
                                                            
                                      Aqui vc tem de tomar uma decisão, caso precise trabalhar com mais de uma viewmodel,
                                      consequentemente  preparar injeção de mais dependencias.
                                      Ou você pode inicializar todos os modulos por aqui ou criar uma classe de inicialização
                                      independente pra cada 1.
                                                            
                                      6)Depois no Android manifest:
                                                            
                                      android:name=".InjecaoDependencias.AplicationKoin"
                                                             
                                      você adiciona o arquivo pra iniciar com o Oncreate.
                                                             
                                      7) Depois de tudo isso configurado é só usar.
                                      Aqui vai uma dica.
                                      Antes de inicializar a viewmodel lançe o import nos imports pra não se incomodar 
                                      escolhendo o componente viewmodel certo. Senão teu fonte pode dar erro e tu nem vai saber porque:
                                                             
                                      Nos imports:
                                                             
                                      import org.koin.androidx.viewmodel.ext.android.viewModel
                                                             
                                                             
                                      Inicializando a viewmodel:
                                      val ListaViewModel: viewmodelEventos by viewModel()
                                                              
                                      E é isso ai, dai é só usar tua viewmodel normalmente e suas funções
                                                            
                                                            
                                                            
