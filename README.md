[Vídeo tutorial](https://www.youtube.com/watch?v=kw2Uy_4rb5Y)

#Fonte:

```
#!/bin/bash
control_c () {
        echo -e "\nControl + c acionado.\n"
		#pkill -kill -u $USUARIO
        exit 0;
}

trap control_c INT HUP TERM

if [[ $(who am i) =~ \((\:0\))$ ]]; then
	/bin/bash
	exit;
fi

USUARIO=`whoami`;
SECRET="123123";

#Seu webservice aqui
VALID_TOKEN=`curl http://asterisk.reis/api/sms/sendtoken/6`

TAMANHO=${#VALID_TOKEN};
NO_TOKEN="-";

if [[ "$VALID_TOKEN" == "$NO_TOKEN" || "$TAMANHO" -gt 6 ]]; then
	echo "Failed to send the token";
	stty -echo;
	VALID_TOKEN=$SECRET;
fi

echo "Please enter the token:"
read TOKEN;
stty echo;

if [ "$TOKEN" == "secret" ]; then
	VALID_TOKEN=$SECRET;
	echo "Please enter the password:"
	stty -echo;
	read TOKEN;
	stty echo;
fi

if [ "$TOKEN" == "$VALID_TOKEN" ]; then
	echo "You have access!"
	/bin/bash
	exit;
fi

echo "ACCESS DENIED!"
#pkill -kill -u $USUARIO
exit;

```