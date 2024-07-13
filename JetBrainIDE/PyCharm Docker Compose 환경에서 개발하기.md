https://www.jetbrains.com/help/pycharm/using-docker-compose-as-a-remote-interpreter.html#configuring-docker
이 문서대로 따라하면 되는데, 하나 중요한건 가상환경을 설정할 경우 System 인터프리터 그대로 설정하는 것이 아니라

https://gist.github.com/jmsalcido/d89f4fa5059867816000220eebe0d95c
여기서 설명하는 것 처럼
`poetry env info -p`로 `venv` 폴더 위치를 찾은 후에, `/bin/python`을 붙인 값을 설정해주어야 한다.