
-- pgcrypto 확장 설치
```
CREATE EXTENSION IF NOT EXISTS pgcrypto;
```

-- INSERT 하면서 자동 BCrypt 인코딩
```
INSERT INTO admins (username, passcode, email) 
VALUES (
    'admin',
    crypt('mySecretPasscode123', gen_salt('bf', 10)),
    'admin@peace.org'
);
```