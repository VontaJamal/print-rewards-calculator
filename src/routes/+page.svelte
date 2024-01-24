<script lang='ts'>
  import { Card, Input, Label, P } from 'flowbite-svelte';

  const ONE_MILLION = 1000000
  const ONE_BILLION = 1000000000

  const solanaPrice = 86
  let volume = 1
  let percentageEarned = .06
  let supply = 10
  let print = 5 * ONE_MILLION
  let totalDailyRewards = '0.00'
  let totalMonthlyRewards = '0.00'

function formatCurrency(amount: number): string {
  if (isNaN(amount)) {
    return 'Invalid input';
  }

  const formattedAmount = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(amount);

  return formattedAmount;
}

  function calculateRewards() {
    //Step 1: Multiply total 24 hour volume by .06 (X value)
    const calculatedVolume = volume * ONE_MILLION
    const percentOfVolume = calculatedVolume * percentageEarned

    //Step 2: Divide total coin in your wallet by total supply of 10b (Y value)
    const calculatedSupply = supply * ONE_BILLION
    const amountOfSupply = print/calculatedSupply

    //Step 3: Calculate rewards
    const rewards = percentOfVolume * amountOfSupply
     totalDailyRewards = formatCurrency(rewards)
     totalMonthlyRewards = formatCurrency(rewards * 30)
    console.log('rewards', rewards)
  }

  calculateRewards()
  $: supply, volume, print, calculateRewards()
</script>

<div class='w-full min-h-screen flex items-center justify-center'>
  <Card>
    <h2 class="mb-5 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Print Rewards Calculator</h2>
      <div class="grid gap-6 mb-6">
    <div>
      <Label for="print" class="mb-2 font-semibold">$PRINT </Label>
      <Input type="number" id="print" placeholder="" required bind:value={print} />
    </div>
    <div>
      <Label for="volume" class="mb-2 font-semibold">24hr Volume (millions)</Label>
      <Input type="number" id="volume" placeholder="" required bind:value={volume} />
    </div>
    <div>
      <Label for="supply" class="mb-2 font-semibold">Supply (billions)</Label>
      <Input type="number" id="supply" placeholder="" required bind:value={supply} />
    </div>
    </div>
    <P size='lg' align='center'>Estimated Daily Print: {totalDailyRewards} 🖨️</P>
    <P size='lg' align='center'>Estimated 30 day Print: {totalMonthlyRewards} 🖨️</P>
  </Card>

</div>
