##Основные компоненты

	Main engine (*.exe) - работа с системными вызовами, связь клиент/сервер.						//Сеть
	Renderer module (renderer.dll) - OpenGl 3.2 + SDL2. Можем рассмотреть вариант с OGRE			//Рендер
	Game module (game.dll) - игровой контент, логика, физика и т.д. . 								//Логика

![](http://s8.hostingkartinok.com/uploads/images/2017/02/e1296810582cb8810756560330b0597f.png)

##Основной цикл

Наш main при этом будет выглядеть примерно так (из оригинального кода Q2):

	WinMain()	//From quake2.exe
	{
	
		Qcommon_Init(argc,argv);
			
		while(1)
		{
			Qcommon_Frame
			{
				SV_Frame()	//Серверная часть
				{
					//Если сетевая часть не выполняет роль сервера
					if(!svs.initialized)
						return;

					//Вызываем game.dll через указатель
					ge->RunFrame();
				}
					
				CL_Frame()	//Клиентская часть
				{
					//Если серверу не надо ничего отрисовывать
					if(dedicated->value)
						return;

					//Вызываем renderer.dll через указатель
					re.BeginFrame();
					//[...]
					re.EndFrame();
				}	
			}
		}
	}

##Два основных преимущества данной архитектуры:
1. Клиент/сервер: нет необходимости в отдельном сервере. Один и тот же экзешник может выполнять роль как клиента, так и сервера. В том числе и одновремено.
2. Модульность: Render module и Game module могут заменяться без изменений в Main engine. 


#Отрисовка.
OpenGL 3.2 поддерживают уже все видеокарты (даже яблочные).

Возможно, изменим координатную систему OGL на более удобную:

    Ось X: Влево/Вправо
    Ось Y: Вперед/Назад		вместо 		Вверх/Вниз
    Ось Z: Вверх/Вниз		вместо		Вперед/Назад

#Дополнительно.

GUI - MyGUI.	
Физика - Bullett.

Остальные инструменты обсудим в конфе.
