# File Documentation: web/src/pages/profile-setting/plan/index.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/plan/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 122
- **Characters**: 3,680
- **Size**: 3,680 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import { Segmented, SegmentedValue } from '@/components/ui/segmented';
import { CircleCheckBig, LogOut } from 'lucide-react';
import { useMemo, useState } from 'react';
import { PricingCard } from './pricing-card';

const pricingData = [
  {
    title: 'Free',
    price: '$0',
    description: 'Meh, just looking',
    features: [
      { name: 'Project', value: '1 project' },
      { name: 'Storage', value: '1 Gb' },
      { name: 'Team', value: '2 members' },
      { name: 'Features', value: 'Basic features' },
    ],
    buttonText: 'Current plan',
    buttonVariant: 'outline' as const,
  },
  {
    title: 'Pro',
    price: '$16.00',
    description: 'For professional use.',
    features: [
      { name: 'Project', value: 'Unlimited projects' },
      { name: 'Storage', value: '100 Gb' },
      { name: 'Team', value: 'Unlimited members' },
      { name: 'Features', value: 'Basic features All advanced features' },
    ],
    buttonText: 'Upgrade',
    buttonVariant: 'default' as const,
    isPro: true,
  },
  {
    title: 'Enterprise',
    price: 'Customed',
    description:
      'Get full capabilities and support for large-scale mission-critical systems.',
    features: [
      { name: 'Project', value: 'Unlimited projects' },
      { name: 'Storage', value: '100 Gb' },
      { name: 'Team', value: 'Unlimited members' },
      { name: 'Features', value: 'Basic features All advanced features' },
    ],
    buttonText: 'Contact us',
    buttonVariant: 'secondary' as const,
    isEnterprise: true,
  },
];

export default function Plan() {
  const [val, setVal] = useState('monthly');
  const options = useMemo(() => {
    return [
      {
        label: 'Monthly',
        value: 'monthly',
      },
      {
        label: 'Yearly',
        value: 'yearly',
      },
    ];
  }, []);

  const handleChange = (path: SegmentedValue) => {
    setVal(path as string);
  };

  const list = [
    'Full access to pro features',
    'Exclusive analyze models',
    'Create more teams',
    'Invite more collaborators',
  ];

  return (
    <section className="p-8">
      <h1 className="text-3xl font-bold mb-6">Plan & balance</h1>
      <Card className="border-0 p-6 mb-6  divide-y divide-colors-outline-neutral-strong">
        <div className="pb-2 flex justify-between text-xl">
          <span className="font-bold ">Balance</span>
          <span className="font-medium">$ 100.00</span>
        </div>
        <div className="flex items-center justify-between pt-3">
          <span>The value equals to 1,000 tokens or 10.00 GBs of storage</span>
          <Button size={'sm'}>
            <LogOut />
            Recharge
          </Button>
        </div>
      </Card>
      <Card className="pt-6 ">
        <CardContent className="space-y-4">
          <div className="font-bold text-xl">Upgrade to access</div>
          <section className="grid grid-cols-2 gap-3">
            {list.map((x, idx) => (
              <div key={idx} className="flex items-center gap-2">
                <CircleCheckBig className="size-4" />
                <span>{x}</span>
              </div>
            ))}
          </section>
          <Segmented
            options={options}
            value={val}
            onChange={handleChange}
            className="inline-flex"
          ></Segmented>
          <div className="grid gap-8 md:grid-cols-2 lg:grid-cols-3">
            {pricingData.map((plan, index) => (
              <PricingCard key={index} {...plan} />
            ))}
          </div>
        </CardContent>
      </Card>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/profile-setting/plan/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 122 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `Plan`: Exported entity

### Functions (3)

- `Plan()`: Function definition
- `options()`: Function definition
- `handleChange()`: Function definition

### Imports (6)

- `import { Button } from '@/components/ui/button';`
- `import { Card, CardContent } from '@/components/ui/card';`
- `import { Segmented, SegmentedValue } from '@/components/ui/segmented';`
- `import { CircleCheckBig, LogOut } from 'lucide-react';`
- `import { useMemo, useState } from 'react';`
- `import { PricingCard } from './pricing-card';`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 6 (4.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~116


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/card`
- `@/components/ui/segmented`
- `lucide-react`
- `react`
- `./pricing-card`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/profile-setting/plan`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/profile-setting/plan/` directory
- Potential test file: `test_index.tsx`

## Keywords

./pricing-card, @/components/ui/button, @/components/ui/card, @/components/ui/segmented, All, Balance, Basic, Button, Card, CardContent, CircleCheckBig, Contact, Create, Current, Customed, Enterprise, Exclusive, Features, For, Free, Full, GBs, Get, Invite, LogOut, Meh, Monthly, Plan, PricingCard, Pro, Project, Recharge, Segmented, SegmentedValue, Storage, Team, The, TypeScript, Unlimited, Upgrade, Yearly, handleChange, list, lucide-react, options, pricingData, react

---
*Generated by RAGFlow Repository Documentation Generator*
