---
aliases:
  - Per-Page Layouts
---
- ページごとに異なるレイアウトを使用する場合に使う
```tsx
// src/app/pages/app/dashboard.tsx
import { ReactElement } from 'react';

import { ContentLayout, DashboardLayout } from '@/components/layouts';

export const DashboardPage = () => {
  ...
  return (
    <>
    ...
    </>
  );
};

DashboardPage.getLayout = (page: ReactElement) => {
  return (
    <DashboardLayout>
      <ContentLayout title="Dashboard">{page}</ContentLayout>
    </DashboardLayout>
  );
};

// pages/_app.tsx
import { NextPage } from 'next';
import type { AppProps } from 'next/app';
import { ReactElement, ReactNode } from 'react';

import { AppProvider } from '@/app/provider';

import '@/styles/globals.css';

// eslint-disable-next-line @typescript-eslint/ban-types
export type NextPageWithLayout<P = {}, IP = P> = NextPage<P, IP> & {
  getLayout?: (page: ReactElement) => ReactNode;
};

type AppPropsWithLayout = AppProps & {
  Component: NextPageWithLayout;
};

export default function App({ Component, pageProps }: AppPropsWithLayout) {
  const getLayout = Component.getLayout ?? ((page) => page);
  return <AppProvider>{getLayout(<Component {...pageProps} />)}</AppProvider>;
}
```