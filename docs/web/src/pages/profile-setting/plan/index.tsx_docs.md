# Documentation: web/src/pages/profile-setting/plan/index.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/plan/index.tsx`
- **Size**: 3680 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/profile-setting/plan/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/profile-setting/plan/index.tsx` is located in the `web/src/pages/profile-setting/plan` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to plan.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [pricing-card.tsx](pricing-card.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
