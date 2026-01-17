# ABox — Руководство по разработке

## Содержание

1. [Введение](#введение)
2. [Требования к окружению](#требования-к-окружению)
3. [Настройка среды разработки](#настройка-среды-разработки)
4. [Структура проекта](#структура-проекта)
5. [Процесс разработки](#процесс-разработки)
6. [Стандарты кодирования](#стандарты-кодирования)
7. [Тестирование](#тестирование)
8. [Работа с Git](#работа-с-git)
9. [Локальный запуск](#локальный-запуск)
10. [Отладка](#отладка)
11. [CI/CD](#cicd)

---

## Введение

Это руководство предназначено для разработчиков, которые хотят внести вклад в проект ABox. Оно описывает процесс настройки среды разработки, стандарты кодирования, процесс работы с Git и другие важные аспекты разработки.

### Целевая аудитория

- Java-разработчики
- Backend-разработчики
- DevOps-инженеры
- Контрибьюторы проекта

---

## Требования к окружению

### Обязательные инструменты

- **Java**: JDK 17 или выше
- **Maven**: 3.8+ или **Gradle**: 7.0+
- **Docker**: 20.10+ и Docker Compose 2.0+
- **Git**: 2.30+
- **IDE**: IntelliJ IDEA / Eclipse / VS Code (рекомендуется IntelliJ IDEA)

### Опциональные инструменты

- **PostgreSQL**: 14+ (для локальной разработки без Docker)
- **Redis**: 6.0+ (для локальной разработки без Docker)
- **Postman / Insomnia**: для тестирования API
- **kubectl**: для работы с Kubernetes (на этапе 2+)

### Проверка установки

```bash
# Проверка Java
java -version

# Проверка Maven
mvn -version

# Проверка Docker
docker --version
docker-compose --version

# Проверка Git
git --version
```

---

## Настройка среды разработки

### 1. Клонирование репозитория

```bash
git clone https://github.com/Pavuch0k/Abox.git
cd Abox
```

### 2. Настройка IDE

#### IntelliJ IDEA

1. Откройте проект: `File → Open → выберите папку Abox`
2. Настройте JDK: `File → Project Structure → Project → SDK: Java 17`
3. Установите плагины:
   - Lombok
   - Spring Boot
   - Docker
   - Kubernetes (опционально)

#### VS Code

1. Установите расширения:
   - Extension Pack for Java
   - Spring Boot Extension Pack
   - Docker
   - GitLens

### 3. Настройка переменных окружения

Создайте файл `.env` в корне проекта (на основе `.env.example`):

```bash
# Database
POSTGRES_DB=abox
POSTGRES_USER=abox_user
POSTGRES_PASSWORD=abox_password

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your-secret-key-change-in-production
JWT_ACCESS_EXPIRATION=900000  # 15 минут
JWT_REFRESH_EXPIRATION=604800000  # 7 дней

# Application
AUTH_SERVICE_PORT=8081
USER_SERVICE_PORT=8082
SESSION_SERVICE_PORT=8083
GATEWAY_PORT=8080
```

---

## Структура проекта

```
Abox/
├── docs/                    # Документация
│   ├── SPECIFICATION.md
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── DEPLOYMENT.md
│   └── DEVELOPMENT.md
├── src/
│   └── java/
│       ├── gateway/         # API Gateway сервис
│       ├── auth-service/    # Сервис аутентификации
│       ├── user-service/    # Сервис управления пользователями
│       ├── session-service/ # Сервис управления сессиями
│       ├── ui-service/      # UI сервис
│       └── metrics-service/ # Сервис метрик
├── docker/                  # Docker конфигурации
│   ├── Dockerfile.gateway
│   ├── Dockerfile.auth
│   └── ...
├── docker-compose.yml       # Локальная разработка
├── docker-compose.prod.yml  # Продакшен конфигурация
├── k8s/                     # Kubernetes манифесты (этап 2+)
├── helm/                    # Helm charts (этап 2+)
├── .github/
│   └── workflows/           # CI/CD workflows
├── .gitignore
├── LICENSE
└── README.md
```

### Структура микросервиса

Каждый микросервис должен следовать стандартной структуре:

```
service-name/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/abox/service/
│   │   │       ├── ServiceApplication.java
│   │   │       ├── config/          # Конфигурации
│   │   │       ├── controller/      # REST контроллеры
│   │   │       ├── service/         # Бизнес-логика
│   │   │       ├── repository/      # Репозитории
│   │   │       ├── model/           # Модели данных
│   │   │       ├── dto/             # Data Transfer Objects
│   │   │       ├── exception/       # Исключения
│   │   │       └── util/            # Утилиты
│   │   └── resources/
│   │       ├── application.yml
│   │       └── application-dev.yml
│   └── test/
│       └── java/
│           └── com/abox/service/
├── Dockerfile
└── pom.xml / build.gradle
```

---

## Процесс разработки

### 1. Выбор задачи

- Проверьте Issues на GitHub
- Выберите задачу или создайте новую
- Обсудите подход с командой перед началом работы

### 2. Создание ветки

```bash
# Создайте ветку от main
git checkout main
git pull origin main
git checkout -b feature/название-функции
# или
git checkout -b fix/описание-бага
# или
git checkout -b docs/описание-изменений
```

### 3. Разработка

- Следуйте стандартам кодирования
- Пишите тесты для нового функционала
- Обновляйте документацию при необходимости
- Делайте частые коммиты с понятными сообщениями

### 4. Тестирование

- Запустите все тесты: `mvn test` или `./gradlew test`
- Проверьте работу локально через Docker Compose
- Протестируйте API через Postman/Insomnia

### 5. Создание Pull Request

- Запушьте ветку: `git push origin feature/название-функции`
- Создайте PR на GitHub
- Заполните шаблон PR
- Дождитесь code review

---

## Стандарты кодирования

### Java Code Style

- **Java Version**: 17+
- **Кодировка**: UTF-8
- **Отступы**: 4 пробела (не табы)
- **Длина строки**: максимум 120 символов
- **Именование**:
  - Классы: `PascalCase` (например, `UserService`)
  - Методы/переменные: `camelCase` (например, `getUserById`)
  - Константы: `UPPER_SNAKE_CASE` (например, `MAX_RETRY_COUNT`)
  - Пакеты: `lowercase` (например, `com.abox.service`)

### Пример кода

```java
package com.abox.auth.service;

import com.abox.auth.model.User;
import com.abox.auth.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;

/**
 * Сервис для управления пользователями.
 */
@Slf4j
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    /**
     * Получает пользователя по ID.
     *
     * @param userId ID пользователя
     * @return пользователь
     * @throws UserNotFoundException если пользователь не найден
     */
    public User getUserById(Long userId) {
        log.debug("Получение пользователя с ID: {}", userId);
        return userRepository.findById(userId)
            .orElseThrow(() -> new UserNotFoundException("User not found: " + userId));
    }
}
```

### Spring Boot Best Practices

- Используйте `@RequiredArgsConstructor` вместо `@Autowired`
- Используйте `@Slf4j` для логирования
- Группируйте конфигурации в `@Configuration` классы
- Используйте DTO для API запросов/ответов
- Валидируйте входные данные через `@Valid`

### Обработка ошибок

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(new ErrorResponse("USER_NOT_FOUND", ex.getMessage()));
    }
}
```

---

## Тестирование

### Типы тестов

1. **Unit тесты**: тестирование отдельных компонентов
2. **Integration тесты**: тестирование взаимодействия компонентов
3. **API тесты**: тестирование REST эндпоинтов

### Структура тестов

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    @DisplayName("Должен вернуть пользователя при существующем ID")
    void shouldReturnUserWhenIdExists() {
        // Given
        Long userId = 1L;
        User user = new User();
        user.setId(userId);
        when(userRepository.findById(userId)).thenReturn(Optional.of(user));
        
        // When
        User result = userService.getUserById(userId);
        
        // Then
        assertThat(result).isNotNull();
        assertThat(result.getId()).isEqualTo(userId);
        verify(userRepository).findById(userId);
    }
}
```

### Запуск тестов

```bash
# Все тесты
mvn test

# Конкретный тест
mvn test -Dtest=UserServiceTest

# С покрытием
mvn test jacoco:report
```

### Требования к покрытию

- Минимум 70% покрытия кода тестами
- Критичные компоненты (auth, security) — минимум 80%

---

## Работа с Git

### Commit Message Format

Используйте формат:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Типы:**
- `feat`: новая функциональность
- `fix`: исправление бага
- `docs`: изменения в документации
- `style`: форматирование кода
- `refactor`: рефакторинг
- `test`: добавление/изменение тестов
- `chore`: обновление зависимостей, конфигураций

**Примеры:**

```
feat(auth): добавлена поддержка refresh токенов

Реализован механизм обновления access токенов через
refresh токены с хранением в Redis.

Closes #123
```

```
fix(user): исправлена валидация email

Email теперь корректно валидируется с учетом
международных доменов.

Fixes #456
```

### Git Workflow

1. **Создайте ветку от main**
2. **Делайте частые коммиты** (логически связанные изменения)
3. **Перед PR**: обновите ветку main и сделайте rebase
4. **После code review**: внесите изменения и сделайте force push (если нужно)

```bash
# Обновление ветки перед PR
git checkout main
git pull origin main
git checkout feature/my-feature
git rebase main

# Разрешение конфликтов (если есть)
# ... исправьте конфликты ...
git add .
git rebase --continue
```

---

## Локальный запуск

### Запуск через Docker Compose

```bash
# Запуск всех сервисов
docker-compose up -d

# Просмотр логов
docker-compose logs -f

# Остановка
docker-compose down
```

### Запуск отдельных сервисов

```bash
# Запуск только инфраструктуры (БД, Redis)
docker-compose up -d postgres redis

# Запуск сервиса локально
cd src/java/auth-service
mvn spring-boot:run
```

### Проверка работы

```bash
# Проверка health endpoints
curl http://localhost:8080/actuator/health

# Проверка регистрации
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

---

## Отладка

### Логирование

Используйте SLF4J + Logback:

```java
@Slf4j
public class MyService {
    public void doSomething() {
        log.debug("Debug message");
        log.info("Info message");
        log.warn("Warning message");
        log.error("Error message", exception);
    }
}
```

### Уровни логирования

В `application-dev.yml`:

```yaml
logging:
  level:
    root: INFO
    com.abox: DEBUG
    org.springframework.web: DEBUG
```

### Отладка в IDE

1. Запустите сервис в режиме Debug
2. Установите breakpoints
3. Используйте Postman для отправки запросов
4. Отслеживайте выполнение через Debugger

### Отладка в Docker

```bash
# Просмотр логов контейнера
docker-compose logs -f auth-service

# Вход в контейнер
docker-compose exec auth-service sh

# Проверка переменных окружения
docker-compose exec auth-service env
```

---

## CI/CD

### GitHub Actions

Проект использует GitHub Actions для:
- Автоматического запуска тестов
- Проверки качества кода
- Деплоя документации
- Сборки Docker образов (в будущем)

### Локальная проверка перед push

```bash
# Запуск тестов
mvn test

# Проверка форматирования
mvn spotless:check

# Исправление форматирования
mvn spotless:apply

# Проверка статического анализа
mvn checkstyle:check
```

---

## Полезные ссылки

- [Техническая спецификация](./SPECIFICATION.md)
- [Архитектура](./ARCHITECTURE.md)
- [API документация](./API.md)
- [Руководство по развертыванию](./DEPLOYMENT.md)
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Docker Documentation](https://docs.docker.com/)

---

## Вопросы и поддержка

Если у вас возникли вопросы:
1. Проверьте документацию
2. Поищите в Issues на GitHub
3. Создайте новый Issue с вопросом
4. Свяжитесь с авторами проекта

---

**Последнее обновление**: 2025-01-17
