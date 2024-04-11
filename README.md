# Frontend Best Practices

# Folder Structure

```
src
|
+-- assets            # assets folder can contain all the static files such as images, fonts, etc.
|
+-- components        # shared components used across the entire application
|
+-- config            # all the global configuration, env variables etc. get exported from here and used in the app
|
+-- constants          # constants 
|
+-- hoc               # higer order components
|
+-- hooks             # shared hooks used across the entire application
|
+-- lib               # re-exporting different libraries preconfigured for the application
|
+-- mock              # mocke data for making static ui.
|
+-- providers         # all of the application providers
|
+-- api               # Rest api's
|
+------ queries       # contains api route to retrive data [GET]
|
+------ mutation      # contains api to modify data [PUT, POST, PATCH, DELETE]
|
+-- routes            # routes configuration
|
+-- stores            # global state stores
|
+-- test              # test utilities and mock server
|
+-- types             # base types used across the application
|
+-- utils             # shared utility functions

```

# Useful sites

| Task | URL |
| --- | --- |
| compress | https://www.iloveimg.com/ |
| favicon generator | https://favicon.io/ |
| table to csv and more conversion | https://tableconvert.com/ |

# Packages for doing shits

| Task | Package | site |
| --- | --- | --- |
| framework | Nextjs,Vite |  |
| alert | Sonner |  |
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
| live share | tunnelmole | https://tunnelmole.com/ |
|next js router events| nextjs-router-events | https://github.com/run4w4y/nextjs-router-events|

# Pattern for designing components

## Twin Merge

```tsx
import { type ClassValue, clsx } from 'clsx';
import tailwindConfig from '../../tailwind.config';
import { extendTailwindMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  const twMerge = extendTailwindMerge({
    // @ts-ignore
    classGroups: {
      // Ensures that the fontSize custom class group is merged correctly
      // Ref: https://github.com/dcastil/tailwind-merge/issues/97
      'font-size': [{ text: Object.keys(tailwindConfig.theme?.extend?.fontSize || []) }],
    },
  });

  return twMerge(clsx(inputs));
}
```

## Block Heading

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

## Container

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

## Providers

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

## Responsive Image

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

## Dividers

```jsx
// Usage
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
    />
  );
});

export type { SeparatorProps };
export { Separator };
```

## Design Guidelines

```tsx
/** @type {import('tailwindcss').Config} */
export default {
  darkMode: ['class'],
  content: ['./pages/**/*.{ts,tsx}', './components/**/*.{ts,tsx}', './app/**/*.{ts,tsx}', './src/**/*.{ts,tsx}'],
  theme: {
    container: {
      center: true,
      padding: '2rem',
      screens: {
        '2xl': '1400px',
      },
    },
    extend: {
      boxShadow: {
        round: '0px 2px 2px 0px rgba(0, 0, 0, 0.10)',
        popup: '1rem 1rem 2rem 0 rgba(0, 0, 0, 0.24)',
      },
      fontFamily: {
        default: ['var(--font-inter)', 'sans-serif'],
        heading: ['var(--font-figtree)', 'sans-serif'],
      },
      spacing: {
        4.5: '1.125rem', //18px
        5.5: '1.375rem', //22px
        7.5: '1.875rem', //30px
        12.5: '3.125rem', //50px
        13: '3.25rem', //52px
        15: '3.75rem', //60px
        17: '4.25rem', //68px
        18: '4.5rem', //72px
        19: '4.75rem', //76px
        21: '5.25rem', //84px
        22: '5.5rem', //88px
        23: '5.75rem', //92px
        25: '6.25rem', //100px
        29: '7.25rem', //116px
        29.5: '7.375rem', //118px
        31: '7.75rem', //124px
        35.5: '8.875rem', //142px
        37: '9.25rem', //148px
        49: '12.25rem', //196px
        56: '14rem', // 224px
        'lg-gutter': '1.875rem', //30px
        'md-gutter': '1.25rem', //20px
        'sm-gutter': '1rem', //16px
      },
      colors: {
        border: 'hsl(var(--border))',
        input: 'hsl(var(--input))',
        ring: 'hsl(var(--ring))',
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
        },
        destructive: {
          DEFAULT: 'hsl(var(--destructive))',
          foreground: 'hsl(var(--destructive-foreground))',
        },
        muted: {
          DEFAULT: 'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        // ? Accents shades
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',

          purple: 'var(--accent-purple)',
          red: 'var(--accent-red)',
          green: 'var(--accent-green)',

          friendship: 'var(--accent-friendship)',
          spiritual: 'var(--accent-spiritual)',
          study: 'var(--accent-study)',
          baptismal: 'var(--accent-baptismal)',
          mentor_and_training: 'var(--accent-mentor-and-training)',

          50: 'var(--accent-50)',
          100: 'var(--accent-100)',
          600: 'var(--accent-600)',
          700: 'var(--accent-700)',
          800: 'var(--accent-800)',
          900: 'var(--accent-900)',
          popover: {
            DEFAULT: 'hsl(var(--popover))',
            foreground: 'hsl(var(--popover-foreground))',
          },
          card: {
            DEFAULT: 'hsl(var(--card))',
            foreground: 'hsl(var(--card-foreground))',
          },
        },
        //? text colors
        content: {
          heading: 'var(--text-heading)',
          subtitle: 'var(--text-subtitle)',
          body: 'var(--text-body)',
          placeholder: 'var(--text-placeholder)',
          disabled: 'var(--text-disabled)',
        },
        day: {
          body: 'rgba(34, 34, 55, 0.74)',
        },
        //? BG colors
        bg: {
          tooltip: 'var(--tooltip-bg)',
          dark: 'var(--dark)',
          white: 'var(--white)',
          disabled: 'var(--disabled)',
          light: {
            grey: 'var(--light-grey)',
          },
        },
        state: {
          success: {
            base: 'var(--state-success-base)',
            light: 'var(--state-success-light)',
            dark: 'var(--state-success-dark)',
          },
          error: {
            base: 'var(--state-error-base)',
            light: 'var(--state-error-light)',
            dark: 'var(--state-error-dark)',
          },
          info: {
            base: 'var(--state-info-base)',
            light: 'var(--state-info-light)',
            dark: 'var(--state-info-dark)',
          },
          warning: {
            base: 'var(--state-warning-base)',
            light: 'var(--state-warning-light)',
            dark: 'var(--state-warning-dark)',
          },
        },
        button: {
          50: 'var(--button-normal-50)',
          100: 'var(--button-normal-100)',
          600: 'var(--button-normal-600)',
          700: 'var(--button-normal-700)',
          800: 'var(--button-normal-800)',
        },
        //? Black shades
        black: {
          100: 'var(--black-100)',
          80: 'var(--black-80)',
          60: 'var(--black-60)',
          40: 'var(--black-40)',
          25: 'var(--black-25)',
          20: 'var(--black-20)',
          10: 'var(--black-10)',
          8: 'var(--black-8)',
          4: 'var(--black-4)',
        },
        //? White shades
        white: {
          100: 'var(--white-100)',
          80: 'var(--white-80)',
          60: 'var(--white-60)',
          40: 'var(--white-40)',
          20: 'var(--white-20)',
          10: 'var(--white-10)',
          8: 'var(--white-8)',
          4: 'var(--white-4)',
        },
        //? Stroke
        stroke: {
          DEFAULT: 'var(--stroke-default)',
          focus: 'var(--stroke-focus)',
          divider: 'var(--stroke-divider)',
        },
        // ? Icon colors
        icon: {
          default: 'var(--icon-default)',
          disabled: 'var(--icon-disabled)',
        },
        scrim: {
          overlay: 'var(--scrim-overlay)',
        },
      },
      borderRadius: {
        Md: 'var(--Radius-Md)',
        Sm: 'var(--Radius-Sm)',
      },
      fontSize: {
        // *=========== HEADINGS START ===========
        h1: [
          '2.625rem',
          {
            lineHeight: '1.2',
            letterSpacing: '-2',
            fontWeight: '700',
          },
        ], //42px
        h2: [
          '2.188rem',
          {
            lineHeight: '1.2',
            fontWeight: '500',
            letterSpacing: '-2',
          },
        ], //35px
        h3: [
          '1.813rem',
          {
            lineHeight: '1.2',
            fontWeight: '500',
            letterSpacing: '-2',
          },
        ], //29px
        h4: [
          '1.5rem',
          {
            lineHeight: '1.2',
            fontWeight: '500',
          },
        ], //24px
        h5: [
          '1.25rem',
          {
            lineHeight: '1.4',
            fontWeight: '500',
          },
        ], //20px
        h6: [
          '1.063rem',
          {
            lineHeight: '1.4',
            fontWeight: '500',
          },
        ], //17px
        b1: [
          '0.875rem',
          {
            lineHeight: '1.4',
            fontWeight: '400',
          },
        ], //14px
        'b1-b': [
          '0.875rem',
          {
            lineHeight: '1.4',
            fontWeight: '600',
          },
        ], //14px semibold
        b2: [
          '0.813rem',
          {
            lineHeight: '1.5',
            fontWeight: '400',
          },
        ], //13px
        'b2-b': [
          '0.813rem',
          {
            lineHeight: '1.5',
            fontWeight: '600',
          },
        ], //13px semibold
        b3: [
          '0.875rem',
          {
            lineHeight: '1.5',
            fontWeight: '400',
          },
        ], //14px
        'b3-b': [
          '0.875rem',
          {
            lineHeight: '1.5',
            fontWeight: '600',
          },
        ], //14px semibold
        c1: [
          '0.75rem',
          {
            lineHeight: '1.3',
            fontWeight: '400',
          },
        ], //12px
        'c1-b': [
          '0.75rem',
          {
            lineHeight: '1.3',
            fontWeight: '600',
          },
        ], //12px semibold
        c2: [
          '0.688rem',
          {
            lineHeight: '1.3',
            fontWeight: '400',
          },
        ], //11px
        'c2-b': [
          '0.688rem',
          {
            lineHeight: '1.3',
            fontWeight: '600',
          },
        ], //11px semibold
        s1: [
          '1.5rem',
          {
            lineHeight: '1.2',
            fontWeight: '400',
          },
        ], //24px semibold
        'bu-l': [
          '1rem',
          {
            lineHeight: '1',
            fontWeight: '600',
          },
        ], //16px semibold
        'bu-m': [
          '0.875rem',
          {
            lineHeight: '1.4',
            fontWeight: '600',
          },
        ], //14px semibold,
        'bu-s': [
          '0.875rem',
          {
            lineHeight: '1.4',
            fontWeight: '600',
          },
        ], //14px semibold
      },
      keyframes: {
        'accordion-down': {
          from: { height: 0 },
          to: { height: 'var(--radix-accordion-content-height)' },
        },
        'accordion-up': {
          from: { height: 'var(--radix-accordion-content-height)' },
          to: { height: 0 },
        },
      },
      animation: {
        'accordion-down': 'accordion-down 0.2s ease-out',
        'accordion-up': 'accordion-up 0.2s ease-out',
      },
      backgroundImage: {
        'auth-background': "url('/assets/bg-auth.png)",
      },
      screens: {
        '2.5xl': '1700px',
        '3xl': '1900px',
      },
    },
  },
  corePlugins: {
    aspectRatio: false,
    container: false,
  },
  plugins: [require('tailwindcss-animate'), require('@tailwindcss/aspect-ratio')],
};
```

## Global CSS

```
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --card: 0 0% 100%;
    --card-foreground: 222.2 84% 4.9%;
    --popover: 0 0% 100%;
    --popover-foreground: 222.2 84% 4.9%;
    --primary: 222.2 47.4% 11.2%;
    --primary-foreground: 210 40% 98%;
    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;
    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;
    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;
    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 40% 98%;
    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;
    --ring: 222.2 84% 4.9%;
    --radius: 0.5rem;
    --Radius-Sm: 0.5rem;
    --Radius-Full: 12.5rem;
    --Radius-Md: 1rem;
    --radius-Sm: 0.5rem;
    /* ? PRIMARY COLOR ACCENTS */
    --accent-purple: #9f1eba;
    --accent-red: #d4145a;
    --accent-green: #22b573;

    --accent-friendship: #223d7c;
    --accent-spiritual: #6f3d18;
    --accent-study: #125e30;
    --accent-baptismal: #fbad1b;
    --accent-mentor-and-training: #ce2032;

    --accent-50: #eaf7fc;
    --accent-100: #bde5f6;
    --accent-600: #29abe2;
    --accent-700: #259acb;
    --accent-800: #1f80aa;
    --accent-900: #0e3c4f;

    /* ? WARNING COLOR ACCENTS */
    --state-warning-base: #fdc854;
    --state-warning-light: #fff9ee;
    --state-warning-dark: #533c09;

    /* ? INFO COLOR ACCENTS */
    --state-info-base: #e7fcff;
    --state-info-light: #fff9ee;
    --state-info-dark: #533c09;

    /* ? ERROR COLOR ACCENTS */
    --state-error-base: #e64c4c;
    --state-error-light: #ffe7e7;
    --state-error-dark: #8c0505;

    /* ? SUCCESS COLOR ACCENTS */
    --state-success-base: #3cc9ae;
    --state-success-light: #e4f5e6;
    --state-success-dark: #00550a;

    /* ? TEXT SHADES */
    --text-heading: #010101;
    --text-subtitle: #010101d6;
    --text-body: #010101bd;
    --text-placeholder: #8a8a8a;
    --text-disabled: #bebebe;

    /* ? BG COLOR */
    --dark: #333333;
    --white: #fafbfc;
    --disabled: #dbdbdb;
    --light-grey: #f7f8fa;
    --tooltip-bg: #333333;

    /* ? BLACK SHADES */
    --black-100: #000000;
    --black-80: #000000cc;
    --black-60: #00000099;
    --black-40: #00000066;
    --black-20: #00000033;
    --black-25: #0000003d;
    --black-10: #0000001a;
    --black-8: #00000014;
    --black-4: #0000000a;

    /* ? WHITE SHADES */
    --white-100: #ffffff;
    --white-80: #ffffffcc;
    --white-60: #ffffff99;
    --white-40: #ffffff66;
    --white-20: #ffffff33;
    --white-10: #ffffff1a;
    --white-8: #ffffff14;
    --white-4: #ffffff0a;

    /* ? BUTTON COLORS */
    --button-normal-50: #eaf7fc;
    --button-normal-50: #bde5f6;
    --button-normal-600: #29abe2;
    --button-normal-700: #259acb;
    --button-normal-800: #1f80aa;

    /* ? STROKE COLORS */
    --stroke-default: #d9e2e8;
    --stroke-focus: #272727;
    --stroke-divider: #dfe3e599;

    /* ? ICON COLORS */
    --icon-default: #3e3d4b;
    --icon-disabled: #a3a3a9;

    /* ? SCRIM COLORS */
    --scrim-overlay: #2727271a;
  }
  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --card: 222.2 84% 4.9%;
    --card-foreground: 210 40% 98%;
    --popover: 222.2 84% 4.9%;
    --popover-foreground: 210 40% 98%;
    --primary: 210 40% 98%;
    --primary-foreground: 222.2 47.4% 11.2%;
    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;
    --muted: 217.2 32.6% 17.5%;
    --muted-foreground: 215 20.2% 65.1%;
    --accent: 217.2 32.6% 17.5%;
    --accent-foreground: 210 40% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 40% 98%;
    --border: 217.2 32.6% 17.5%;
    --input: 217.2 32.6% 17.5%;
    --ring: 212.7 26.8% 83.9%;
  }
}
```

## Email template for responsive mobile

```

<!doctype html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    /* Styles for all clients */
    @media only screen and (max-width: 600px) {
        table {
            width: 100% !important;
        }
        td {
            padding: 10px !important;
        }
        img {
            max-width: 100% !important;
            height: auto !important;
        }
        a {
            display: block !important;
            margin-bottom: 10px !important;
        }
    }
</style>

<!--[if mso]>
<style>
    /* Outlook-specific styles */
    @media only screen and (max-width: 600px) {
        table {
            width: 100% !important;
        }
        td {
            padding: 10px !important;
        }
        img {
            max-width: 100% !important;
            height: auto !important;
        }
        a {
            display: block !important;
            margin-bottom: 10px !important;
        }
    }
</style>
<![endif]-->
</head>

<body style="
      margin: 0;
      padding: 0;
      font-family: &quot;Open Sans&quot;, sans-serif;
    ">
  <table align="center" border="0" cellpadding="0" cellspacing="0" style="border-collapse: collapse; max-width: 700;">
    <tr>
      <td bgcolor="white" style="padding: 36px 48px">
        <div>
          <!-- Logo Section -->
          <img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/logo_with_letter.png"
            alt="One Accord Logo" style="margin-left: 50px" />
        </div>
        <br />
        <!-- Divider -->
        <hr style="border: 1px solid white; margin: 0" />

        <!-- Content Section -->
        <div style="padding: 40px 48px; background: white">
          <div style="
                font-size: 16px;
                font-weight: 700;
                color: #321d36;
                line-height: 22.4px;
              ">
            Hello {{.ChurchName}},
          </div>

          <div style="
                margin-top: 16px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Welcome to the One Accord family. We’re thrilled that you’ve
            decided to join us on our mission to revolutionize the way we
            connect with the community around us.
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            We believe One Accord will help you make a huge impact and can
            hardly wait to see what you will achieve!
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            <strong>We’re excited to announce that your new local church website is
              live and ready to share with the world!
            </strong>
          </div>

          <!-- Button Section -->
          <div style="margin-top: 20px; text-align: center">
            <a href="https://{{.ChurchDomain}}" style="
                  display: inline-block;
                  padding: 10px 20px;
                  background: #29abe2;
                  color: white;
                  text-decoration: none;
                  border-radius: 100px;
                  font-size: 14px;
                  font-weight: 600;
                  line-height: 19.6px;
                ">
              My Website
            </a>
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Your new website also comes with a custom content management
            system so that you can make it your own. Try adding an image of
            your local church or customizing the description on the “About Us’
            page. You can also set your service times, add upcoming events,
            schedule a livestream, and much more.
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            To get started, simply click the button below to set your password
            and you're ready to go! (*If you pastor multiple churches, you
            only need to register once and all your churches should be visible
            in your one account.)
          </div>

          <!-- Button Section -->
          <div style="margin-top: 10px; text-align: center">
            <a href="{{.Link}}" style="
                  display: inline-block;
                  padding: 10px 20px;
                  background: #29abe2;
                  color: white;
                  text-decoration: none;
                  border-radius: 100px;
                  font-size: 14px;
                  font-weight: 600;
                  line-height: 19.6px;
                ">
              Start Here
            </a>
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Once you’ve added your special touch, it’s time to share it with
            your community! Try sharing it on social media or adding it to the
            literature you hand out.
          </div>

          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Make sure to mark your calendars for one of our upcoming webinars,
            where we’ll walk through every aspect of the software and answer
            any questions you might have.
          </div>
          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Thank you for joining us as we try and reach as many as we can in
            this world.
          </div>
          <div style="
                margin-top: 20px;
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 400;
                line-height: 22.4px;
              ">
            Let’s finish the work together!
          </div>
        </div>

        <!-- Footer Section -->
        <div style="padding: 20px 48px">
          <div style="
                color: rgba(1, 1, 1, 0.74);
                font-size: 16px;
                font-weight: 500;
                line-height: 22.4px;
              ">
            Best Regards,<br />
          </div>
          <div style="
                color: #010101;
                font-size: 16px;
                font-weight: 500;
                line-height: 22.4px;
              ">
            One Accord Team
          </div>
          <div style="
                margin-top: 10px;
                color: rgba(1, 1, 1, 0.54);
                font-size: 13px;
                font-weight: 400;
                line-height: 18.2px;
              ">
            If, by any chance, you have received this email in error, please
            disregard it. We apologize for any confusion and appreciate your
            understanding.
          </div>
        </div>
      </td>
    </tr>

    <tr>
      <td align="center">
        <img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/logo.png" alt="One Accord Logo" />
      </td>
    </tr>
    <!-- Social Media Section -->
    <tr>
      <td align="center" bgcolor="white" style="padding: 10px 48px">
        <a href="#"><img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/insta.png"
            alt="Instagram Logo" style="margin-right: 10px" /></a>
        <a href="#"><img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/twitter.png"
            alt="Twitter Logo" style="margin-right: 10px" /></a>
        <a href="#"><img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/fb.png" alt="Facebook Logo"
            style="margin-right: 10px" /></a>
        <a href="#"><img src="https://oneaccord-prod.s3.us-east-2.amazonaws.com/public/email/yt.png"
            alt="Youtube Logo" /></a>
      </td>
    </tr>

    <!-- Footer Info Section -->
    <tr>
      <td align="center" bgcolor="white" style="padding: 20px 48px">
        <div style="
              color: rgba(1, 1, 1, 0.74);
              font-size: 13px;
              font-weight: 400;
              line-height: 16.9px;
            ">
          © One Accord 2024
        </div>
      </td>
    </tr>
  </table>
</body>
 </html>

```

```
git reset --hard origin/dev     
```


FAQS

| Issue | css | tailwind |
| --- | --- | --- |
| image stretch out | object-fit: cover; | object-cover |
|  |  |  |

# Additional resources

https://alexkondov.com/tao-of-react/

https://github.com/ryanmcdermott/clean-code-javascript

Fun fact: React is not built for web

[https://www.youtube.com/watch?v=Y12sGu8-qFE&ab_channel=Theo-t3․gg](https://www.youtube.com/watch?v=Y12sGu8-qFE&ab_channel=Theo-t3%E2%80%A4gg)
