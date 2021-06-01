# A community developed list to spot honeypots & rug pulls

## Checks

### The Contract
- Check to see if contract is using solidity version is 0.5.17 - historically contracts on this version often are rug pulls.
- If you see "submitted for verification" in the comments section of the contract _two or more times_, more than likely the code for that contract was copy and pasted. This is definitely a big indicator for a rug pull.
- Look in the comments section for social media, telegram, and other relevant links.
- Big plus if they put the time and effort to put uscii pictures/logos/etc into the comments section of their contract.

### Ownership
Is ownership renounced? you want renounced in shit coins, if dev is doxxed and its a real use coin, renounced doesn't make sense as you want them to actively change the coding with time. You're looking for something like this:
![Ownership Renounced](https://github.com/broskees/community-checks-list/blob/main/Screen%20Shot%202021-06-01%20at%203.35.40%20PM.png?raw=true)

### Liquidity Pool & Holders
- First check if the tokens were burned. If the majority of the tokesn are in 0x000...000dead or 0x000..., that means the liquidity has been burned.
- If the tokens were not burned, check to see if they're locked. You can find this info by checking dxsale or looking up the token address in the liquidity lockers below.
- Check the percentage of tokens burned/locked. Is it under 90%? If it's under 90% burned/locked and there are one to a few wallets that own the majority of remaining tokens by a large margin, or its a small coin without much PCS to buy from (could mean the wallets are spread out) then it could be a rug pull.
- If they haven't locked the tokens yet, it could take a little while if the token _just launched a few minutes ago_. I suggest waiting until it's properly locked before investing.

### Other Rug Indications
- Check to see if there are any sell orders on poocoin charts. If nobody can sell, it's definitely a rug.
- Check the comments section on bscscan. Did anyone leave a comment alerting to this token being a rug?
- Bad names - TikTok, pornhub, random things found around the house, etc.
- Check if googletagmanager.com is in use on the website. If other websites use the same google analytics profile, be suspicious, especially if they share the same analytics with other past scams. 
  1. (check the code for gtag.js)
  2. Copy the GOOGLE analytics ID (starts with UA)
  3. Go to dnslytics.com. 

### Publicity
- Check out the telegram group: How many members are there? How are the admins behind the coin? Are they behaving professionally and are they handling things well in their telegram group? Do they answer questions?
- Website Quality: Was there care and effort put into the website?
- Social Media: How many followers are there? Do their posts get engagement?

## Tools

### Deep Dive Info
- [bscscan](https://bscscan.com/)
- [bitquery explorer](https://explorer.bitquery.io/bsc/token/0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c)

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
