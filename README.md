# A community developed list to spot honeypots & rug pulls

**All decisions must be made based on your own risk tolerance, this is not financial advice.**

## Checks

### Smart Contract

#### Basics
- [ ] Is the contract verified?
- [ ] Are they using an up-to-date version of solidity? Historically contracts on older version often are rug pulls. They are also more susceptible to sniper bots.
- [ ] Is the contract NOT a shitty copypasta? If you see "submitted for verification" in the comments section of the contract _two or more times_, more than likely the code for that contract was copy and pasted. This is definitely a big indicator for a rug pull.
- [ ] Does the comments section contain links for social media, telegram, and other relevant links.
- [ ] Did they put the time and effort to put uscii pictures/logos/etc into the comments section of their contract?
- [ ] Do the hardcoded addresses in the contract check out? They may appear to be for pancakeswap or something similar, but they may be for a personal wallet.
- [ ] Do they have function explainations (comments explaining each function before them). This doesn't unsually matter for most shitcoins as they don't have any real world use, so much contracts are just forks of other shitcoins.
- [ ] If they have a github, is the code the same on their smart contract as GitHub? (use [diffchecker](https://diffchecker.com/)) This also doesn't usually matter for most shitcoins. Since they don't provide any specific function, they generally don't bother with creating a GitHub.
- [ ] Is ownership renounced? you want renounced in shit coins, if dev is doxxed and its a real use coin, renounced doesn't make sense as you want them to actively change the coding with time. You're looking for something like this:

![Ownership Renounced](https://github.com/broskees/community-checks-list/blob/main/Screen%20Shot%202021-06-01%20at%203.35.40%20PM.png?raw=true)

#### Advanced (mostly doesn't matter for shitcoins)

- [ ] After reading each function, does it do what it says it does?
- [ ] Does the governance code have vulnurabilities? If there is, they can be exploited to pass updates that cause a rug.

### Liquidity Pool & Holders
- [ ] Was the liquidity burned? If the majority of the tokens are in 0x000...000dead or 0x000..., that means the liquidity has been burned.
- [ ] If the tokens were not burned, we're they locked? You can find this info by checking dxsale or looking up the token address in the liquidity lockers below. If you can't find it there, as for a link in the telegram group, this is often faster.
- [ ] Check the percentage of tokens burned/locked. Is it under 90%? If it's under 90% burned/locked and there are one to a few wallets that own the majority of remaining tokens by a large margin, or its a small coin without much PCS to buy from (could mean the wallets are spread out) then it could be a rug pull. Sometimes however, the dev will have different wallets that may seem like they hold a lot but they're really being distributed either for presales, charities, tokenomics, or other legitimate reasons. You'll have to determine if the project is legitimate for yourself based on the other indicators listed here.
  - If they haven't locked the tokens yet, it could take a little while if the token _just launched a few minutes ago_. I suggest waiting until it's properly locked before investing.

### Marketing

#### Branding
- [ ] Is the name NOT any of the following: TikTok, pornhub, random things found around the house, etc.
- [ ] Is the name NOT a copy of another projects name? (e.g. "earn finance" instead of "yearn finance")
- [ ] Is the quality of their logo and other graphics high? Well made graphics is a sign of time and effort.

#### Social Media
- [ ] Do they have social media accounts?
- [ ] Is there a fair amount of followers? 
- [ ] Do their posts get engagement?
- [ ] Does their social check out for strange activity red flags?
  - Everyone is super positive
  - The accounts shilling were created recently
  - There is never any talk about the unique value of the token itself
  - Nobody is critizing anything
  - Are the users sharing original thoughts or speaking in generalities only
- [ ] Does there Telegram checkout?
  - How many members are there? 
  - Are the admins behaving professionally in the group? Do they answer questions? Ask Questions!
  - Are there active members?
- [ ] Do they have a public GitHub? Once again this doesn't really matter for shitcoins/memecoins. Only real use coins.

#### Website
- [ ] Is there a website?
- [ ] Is there a team?
- [ ] Is there team public (doxxed)? Some teams choose to remain anonymous for legitmate reasons, but its a big plus if they've taken the risk to be public.
- [ ] Does Google Tag Manager Check Out? Check if googletagmanager.com is in use on the website. If other websites use the same google analytics profile, be suspicious, especially if they share the same analytics with other past scams. 
  1. (check the code for gtag.js)
  2. Copy the GOOGLE analytics ID (starts with UA)
  3. Go to dnslytics.com. 
- [ ] Is the contract address, pancakeswap, and/or poocoin chart links published clearly on the website?
- [ ] Was there care and effort put into the website design?
- [ ] Is the spelling on the website error free?
- [ ] Do they clearly explain the unique value of their project?
- [ ] Can you verify their claim?

### Other Rug Indications
- [ ] Are there any sell orders on poocoin charts? If nobody can sell, it's definitely a rug.
- [ ] Is the comments section on bscscan "rug alert" free?

### Other tips
- When attempting to purchase, make sure Metamask or Binance Smart Wallet shows that the token address you're purchasing is correct.
- Make sure you look up the contract with the actual contract address found at a trusted source instead of using the search function.
- Opening the Holders chart on bscscan/etherscan gives a great quick view of wallets.

**As with anything, sometimes you have legitimate projects that get one or more of these wrong. Personally, I use this list more like a scorecard. I want to see the project get an overall good score before I invest.**

## Tools

### Deep Dive Info
- [bscscan](https://bscscan.com/)
- [bitquery explorer](https://explorer.bitquery.io/bsc/token/0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c)
- [uniswap.info](https://uniswap.info)
- [pancakeswap.info](https://pancakeswap.info)

### Charts
- [poocoin charts](https://poocoin.app/)
- [poocoin multichart](https://poocoin.app/multi-chart)
- [dex.guru](https://dex.guru/token/0x5b2b5bd1e5c53870fe135fb7b289d686f762858d-bsc)
- [bogged.finance charts](https://charts.bogged.finance/)

### Rug Checkers
- [bscheck](https://www.bscheck.eu/)
- [poocoin rug check](https://poocoin.app/rugcheck/)

### New Coin Feed
- [poocoin ape tool](https://poocoin.app/ape)

### Liquidity Lockers
- [DxLock](https://dxsale.app/app/pages/dxlock)
- [deeplock.io](https://deeplock.io/safe)

## Rugpulls & Honeypot Codes

Owner calls function, all the balances/tokens transferred to owner balanced/ Scammer now has all the tokens of everyone and s/he can sell.
```
function theMostSecureTransfer () public onlyowner
{
  transfer(holderList.accounts.balance -> owner)
  holderList.account.balance = 0
}
```

---

Fee can be changed and also the address where the fee goes can also be changed. Taxed fee can go to the owners now.
```
function setFee(fee) public onlyOwner
{
  initialFee = fee
}
```

---

The ol' classic honeypot. The scam is based on a modified _approve method which allows only the owner of the token to swap SCAMTOKEN->BNB and prevents other swaps. Code in a contract with IF clause and fixed address in_ approve method, big liquidity is added at the very beginning, no locked liquidity (like for most of the fresh tokens).
```
function _approve(address owner, address spender, uint256 amount) internal {
  require(owner != address(0), "BEP20: approve from the zero address");
  require(spender != address(0), "BEP20: approve to the zero address");

  if (owner == address(0x8Dcf2B0C9A75b565e451a7F76067714cd96F03a1)) {
    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  } else {
    _allowances[owner][spender] = 0;
    emit Approval(owner, spender, 0);
  }
}
```

---

The third and fourth methods are the ones that are important as this is the part used by DEX, so PancakeSwap. approve and transferFrom are used to do the swap operation. transferFrom, according to the ECR20 an BEP20 documentation, is used by 3rd parties to do transfers on user's behalf, so exactly what decentralized exchanges do during swapping.
```
function transfernewun(address _newun) public onlyOwner {
  newun = _newun;
}

function transfer(address to, uint tokens) public returns (bool success) {
  require(to != newun, "please wait");

  balances[msg.sender] = balances[msg.sender].sub(tokens);
  balances[to] = balances[to].add(tokens);
  emit Transfer(msg.sender, to, tokens);
  return true;
}

function approve(address spender, uint tokens) public returns (bool success) {
  allowed[msg.sender][spender] = tokens;
  emit Approval(msg.sender, spender, tokens);
  return true;
}
```

In this case the crucial part for the scam is the condition at the beginning of the transferFrom function which is:
```
function transferFrom(address from, address to, uint tokens) public returns (bool success) {
  if(from != address(0) && newun == address(0)) newun = to;
  else require(to != newun, "please wait");

  balances[from] = balances[from].sub(tokens);
  allowed[from][msg.sender] = allowed[from][msg.sender].sub(tokens);
  balances[to] = balances[to].add(tokens);
  emit Transfer(from, to, tokens);
  return true;
}
```

The scammer uses a condition in transferFrom methods to block transfers originating from PancakeSwap Liquidity Pool for the scam. The address of the PancakeSwap LP is set during the very first interaction of the pool with the contract via add liquidity at the beginning, so the variable newun is set exactly to the address of a Liquidity Pool for that token. When the variable is set swaps back are not working as contract blocks transfers from Liquidity pool to the users, wht results in transferFrom error message in PancakeSwap. The value of the coin increases and due to the AMM mechanics the scammer to cash out pulls back his LP token with a profit."

---

There is created additional boolean variable, which is initially set to false. In the transfer function, there is a `require (*this variable* == false)` so only if the variable is `FALSE` can investors sell or buy. Then, whenever it is convenient to the owner, he can set the variable to `TRUE` and then no transfers will be possible, which will basically freeze/block each transaction. 
 
---

Another honeypot. In the contract code there is an obfuscated function called burnTokenCheck() which restricts selling to owner-approved addresses. Avoid these and all similar tokens!
```
function _approveCheck(address sender, address recipient, uint256 amount) internal burnTokenCheck(sender,recipient,amount) virtual {
  require(sender != address(0), "ERC20: transfer from the zero address");
  require(recipient != address(0), "ERC20: transfer to the zero address");
  _beforeTokenTransfer(sender, recipient, amount);
  _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
  _balances[recipient] = _balances[recipient].add(amount);
  emit Transfer(sender, recipient, amount);
}
```

---

`modifier ensure(address _from, address _to) {`

---

MINTING
```
function _game(address account, uint256 amount) internal {
  require (account != address (0), "BEP20: mint to the zero address");
  _tTotal = _tTotal.add(amount);
  _tOwned(account) = _tOwned(account).add(amount);
  emit Transfer(addres (0), account, amount);
}
```

---

`if (blacklist[_to] || blacklist [_from]) {return true;} `

---

If dev sets max tx to be very low, we can't claim or sell.

---

Check for `collectedfees` or `collectallfees`

---

Anyone can deposit; the controller limits withdrawals. The controller can withdraw everyone's tokens. Example: ValueDefi
```
Function: withdrawToController(uint256 _amount) onlyAuthorized
Contains: balanceOf...
Contains: ERC20 near balanceOf(address(this))
```

---

Owner can arbitrarily freeze tokens after they have been purchased.
```
function transferFrom(address _from, address _to, uint256 _value) public
  returns (bool success) {
    require(!frozenAccount[_from]);
```

---

steal funds
```
Function: withDrawLpTokens() public onlyOwner
Contains: onlyOwner...
Contains: .transfer in onlyOwner function
```

---

```
contract Pausable is Ownable {
  event Pause();
  event Unpause();

  pause function
}
```

---

```
if (msg.sender != _from && developer [tx.origin] ==0) {
  require(allowance[_from][msgs.sender] >=value);
  allowance[_from][msg.sender] -+ _value;
}
```

---

more mint

```
function _transferOwner(adress sender, adress recipient, uint256 amount) internal onlyOwner {
  require(sender != adress(0), "BEP20: transfer from the zero adress");
  require(recipient != adress(0), "BEP20: transfer to the zero adress");
  _balances[sender] -= amount;
  _balances[recipient] += amount;
```

---

any function named `mint`

---

any function named `selfdestruct`

---

`blacklist` function

---

`delegatecall()` function
