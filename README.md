# Folder Pattern

`├── modules
|   ├── common
|   |   ├── components
|   |   |   ├── Button.jsx
|   |   |   ├── Input.jsx
|   ├── dashboard
|   |   ├── components
|   |   |   ├── Table.jsx
|   |   |   ├── Sidebar.jsx
|   ├── details
|   |   ├── components
|   |   |   ├── Form.jsx
|   |   |   ├── ItemCard.jsx`

Useful sites 

| Task | URL |
| --- | --- |
| compress | https://www.iloveimg.com/ |
| favicon generator | https://favicon.io/ |
| table to csv and more conversion | https://tableconvert.com/ |

# Packages for doing shits

| Task | Package | site |
| --- | --- | --- |
| framework | Nextjs,Vite |  |
| api | axios with tankstack react query |  |
| form | react hook form with zod |  |
| table | tankstack react table |  |
| Global state manager | zustand |  |
| ui | shadcn |  |
| charts | recharts |  |
| Carousel | swiper | https://swiperjs.com/react |
| animation | framer motion |  |
| date and time | date-fns |  |
| markdown editor | react-quill |  |
|  |  |  |

# Block Heading

```tsx
import * as React from 'react';

import { cn } from '@/utils/shade-cn';

type HeadingType = 'h1' | 'h2' | 'h3' | 'h4' | 'h5';

interface BlockHeadingProps extends React.ComponentPropsWithRef<HeadingType> {
  className?: string;
  type: HeadingType;
  hasLeftBorder?: boolean;
  hasBottomBorder?: boolean;
}

const BlockHeading = React.forwardRef<HTMLHeadingElement, BlockHeadingProps>(
  (
    {
      type: Tag,
      hasLeftBorder = true,
      hasBottomBorder = false,
      className,
      ...props
    },
    ref,
  ) => {
    return (
      <Tag
        ref={ref}
        className={cn([
          'relative text-content-heading',
          // ? text sizes styles
          Tag === 'h1' && 'text-h2 md:pl-8 md:text-h3 lg:text-h1',
          Tag === 'h2' && 'text-h5 md:text-h4 lg:text-h2',
          Tag === 'h3' && 'text-h5 md:text-h4 lg:text-h3',
          Tag === 'h4' && 'text-h5 lg:text-h4',
          Tag === 'h5' && 'text-h6 lg:text-h5',
          hasLeftBorder &&
            'pl-6 before:absolute before:left-0 before:top-0 before:h-full before:w-1 before:bg-accent-400',
          hasBottomBorder && 'border-b border-[#E6E9E7] pb-5',
          className,
        ])}
      >
        {props.children}

        {/* block style */}
      </Tag>
    );
  },
);

BlockHeading.displayName = 'BlockHeading';

export default BlockHeading;
```

# Container

```tsx
import * as React from 'react';

import { cn } from '@/utils/shade-cn';

interface ContainerProps extends React.ComponentPropsWithRef<'div'> {
  gridLayout?: boolean;
  fluid?: boolean;
}

const Container = React.forwardRef<HTMLDivElement, ContainerProps>(
  ({ className, gridLayout, fluid, children, ...props }, ref) => {
    return (
      <div
        className={cn([
          !fluid &&
            'mx-auto w-full max-w-[calc(1320px+32px)] px-layout-margin-sm md:px-layout-margin-md lg:px-layout-margin-lg 2xl:max-w-[77rem] 3xl:max-w-[89.5rem] 4xl:max-w-[1832px]',
          gridLayout && 'grid grid-cols-6 gap-x-5 md:grid-cols-12 lg:gap-x-4.5',
          className,
        ])}
        ref={ref}
        {...props}
      >
        {children}
      </div>
    );
  },
);

Container.displayName = 'Container';

export default Container;
```

# Providers

```tsx
'use client';

import React from 'react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

interface Providers {
  children?: React.ReactNode;
}

// Create a client
const queryClient = new QueryClient();

const Providers: React.FC<Providers> = (props) => {
  return (
    <QueryClientProvider client={queryClient}>
      {props.children}
    </QueryClientProvider>
  );
};

export default Providers;
```

# Responsive Image

```tsx
<div className='aspect-h-[619] aspect-w-[1350] relative mt-10 w-full rounded-lg'>
        <Image
          src={AboutSectionCover}
          alt='Church Overview Image'
          className='flex-shrink-0 rounded-lg object-cover'
          fill
        />
</div>
```

# Dividers

```jsx
<Separator orientation='horizontal' className='my-5' />
```

```jsx
import * as React from 'react';

import { cn } from '@/lib/cn';

interface SeparatorProps {
  orientation?: 'horizontal' | 'vertical';
  className?: string;
}

const Separator = React.forwardRef<HTMLDivElement, SeparatorProps>(({ orientation, className, ...props }, ref) => {
  return (
    <div
      ref={ref}
      className={cn([
        'inline-block whitespace-pre',
        'shrink-0 bg-stroke',
        orientation === 'horizontal' ? 'h-[0.063rem] w-full' : 'h-full w-[0.063rem]',
        className,
      ])}
      {...props}
    >
      {' '}
    </div>
  );
});

export type { SeparatorProps };
export { Separator };
```

# Additional resources

https://alexkondov.com/tao-of-react/

Fun fact: React is not built for web

[https://www.youtube.com/watch?v=Y12sGu8-qFE&ab_channel=Theo-t3․gg](https://www.youtube.com/watch?v=Y12sGu8-qFE&ab_channel=Theo-t3%E2%80%A4gg)
