
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Descargando proyecto Flask'
                checkout scm
            }
        }

        stage('Instalar dependencias') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python -m pip install \
                        -r requirements.txt
                '''
            }
        }

        stage('Validar codigo') {
            steps {
                sh '''
                    .venv/bin/python -m py_compile app.py
                    echo "Sintaxis Python correcta"
                '''
            }
        }

        stage('Pruebas Flask') {
            steps {
                sh '''
                    .venv/bin/python - <<'PY'
from app import app

client = app.test_client()

response = client.get('/hello?name=Duoc')

assert response.status_code == 200
assert response.get_data(as_text=True) == 'Hello, Duoc!'

print('Prueba HTTP Flask exitosa')
PY
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline Flask completado'
        }

        failure {
            echo 'Pipeline con errores'
        }
    }
}
