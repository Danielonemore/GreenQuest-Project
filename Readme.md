-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
INTRUÇÕES PARA TESTAR E APLICAR A API TANTO NO EMULADOR QUANTO NO DISPOSITIVO FÍSICO
//API Local com Emulador
//Android (Emulador): Para testar a API no emulador Android, é necessário usar http://10.0.2.2:3000 no lugar de localhost, já que o emulador não consegue acessar diretamente o localhost da sua máquina. 
//Certifique-se de que o servidor Express está rodando na sua máquina (ou seja, yarn start ou node index.js, dependendo da configuração do seu backend).

//Aqui o exemplo!!!!!
//const quizResponse = await axios.get('http://10.0.2.2:3000/api/quiz');
//const envResponse = await axios.get('http://10.0.2.2:3000/api/environment');
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
//API Local com Dispositivo Físico
//Quando testar com um dispositivo real conectado via USB, substitua localhost pelo IP da sua máquina na rede local. 
//Você pode usar ipconfig (Windows) ou ifconfig (Linux/Mac) para descobrir o IP da sua máquina, e então ajustar as chamadas da API:

//const quizResponse = await axios.get('http://192.168.x.x:3000/api/quiz');
//const envResponse = await axios.get('http://192.168.x.x:3000/api/environment');
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
//Verificando a API no Postman/Insomnia
//Antes de testar no app, pode ser útil garantir que as APIs estão funcionando corretamente utilizando ferramentas como Postman ou Insomnia:

//Inicie seu servidor local (yarn start).
//Em Postman/Insomnia, faça uma requisição GET para http://localhost:3000/api/quiz e http://localhost:3000/api/environment para garantir que você está recebendo as respostas esperadas.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

// O SCRIPT UTILIZANDO O CONSUMO DE API'S
->//API 1: Perguntas de Quiz Ambiental
->//API 2: Dados Ambientais Relevantes
->//Cria uma pasta para o projeto e instala as dependências:
mkdir greenquest-api
cd greenquest-api
yarn init -y
yarn add express


//Consumir as APIs locais:
import axios from 'axios';
import { useState, useEffect } from 'react';
import { View, Text, ScrollView } from 'react-native';

const ApiComponent = () => {
    
  // Estados para armazenar os dados de ambas as APIs
  const [quizData, setQuizData] = useState(null); // Dados do Quiz (API 1)
  const [environmentData, setEnvironmentData] = useState(null); // Dados Ambientais (API 2)
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Função para fazer as chamadas às APIs
  const fetchApis = async () => {
    try {
      // Chamada à primeira API (quiz)
      const quizResponse = await axios.get('http://localhost:3000/api/quiz');
      setQuizData(quizResponse.data); // Armazena os dados do quiz no estado

      // Chamada à segunda API (dados ambientais)
      const envResponse = await axios.get('http://localhost:3000/api/environment');
      setEnvironmentData(envResponse.data); // Armazena os dados ambientais no estado
    } catch (error) {
      setError('Erro ao carregar os dados'); // Em caso de erro
    } finally {
      setLoading(false); // Encerra o carregamento
    }
  };

  // useEffect para fazer a chamada quando o componente é montado
  useEffect(() => {
    fetchApis();
  }, []);

  // Exibir mensagem de carregamento ou erro
  if (loading) return <Text>Carregando...</Text>;
  if (error) return <Text>{error}</Text>;

  // Renderizar os dados recebidos de ambas as APIs
  return (
    <ScrollView>
      <View>
        {/* Exibir os dados do quiz */}
        <Text style={{ fontWeight: 'bold' }}>Perguntas do Quiz:</Text>
        {quizData && quizData.map((item, index) => (
          <Text key={index}>{item.question}</Text>
        ))}

        {/* Exibir os dados ambientais */}
        <Text style={{ fontWeight: 'bold' }}>Dados Ambientais:</Text>
        {environmentData && (
          <Text>{JSON.stringify(environmentData)}</Text>
        )}
      </View>
    </ScrollView>
  );
};

export default ApiComponent;



-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
COMO TESTAR A API NO DISPOSITIVO FÍSICO OU NO EMULADOR
//Como testar a API em um emulador:
//Para um emulador Android, substitui o localhost por 10.0.2.2. Isso redireciona as chamadas do emulador para o localhost do seu computador.
const quizResponse = await axios.get('http://10.0.2.2:3000/api/quiz');
const envResponse = await axios.get('http://10.0.2.2:3000/api/environment');

//Como testar a API em um dispositivo físico:
//Para testar em um dispositivo físico (conectado via USB), precisa usar o IP da sua máquina local no lugar de localhost ou 10.0.2.2. Para encontrar o IP da tua máquina, podes executar o seguinte comando no terminal (no Windows, Linux ou Mac):

Windows: ipconfig
Linux/Mac: ifconfig

//Procurar o IP local da sua rede (algo como 192.168.x.x). Substitui localhost pelo IP da sua máquina:
const quizResponse = await axios.get('http://192.168.x.x:3000/api/quiz');
const envResponse = await axios.get('http://192.168.x.x:3000/api/environment');