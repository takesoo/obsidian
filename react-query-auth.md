---
tags:
  - npm
  - React
---
- [alan2207/react-query-auth: ⚛️ Authenticate your react applications easily with react-query.](https://github.com/alan2207/react-query-auth)
- ユーザー認証のステート管理ライブラリ
```jsx
// lib/auth.tsx
import { configureAuth } from 'react-query-auth';

export const { useUser, useLogin, useRegister, useLogout } = configureAuth({
  userFn: () => api.get('/me'),
  loginFn: (credentials) => api.post('/login', credentials),
  registerFn: (credentials) => api.post('/register', credentials),
  logoutFn: () => api.post('/logout'),
});

// components
import { useUser, useLogout } from '@/lib/auth';

function UserInfo() {
  const user = useUser();
  const logout = useLogout();

  if (user.isLoading) {
    return <div>Loading ...</div>;
  }

  if (user.error) {
    return <div>{JSON.stringify(user.error, null, 2)}</div>;
  }

  if (!user.data) {
    return <div>Not logged in</div>;
  }

  return (
    <div>
      <div>Logged in as {user.data.email}</div>
      <button disabled={logout.isLoading} onClick={logout.mutate({})}>
        Log out
      </button>
    </div>
  );
}
```