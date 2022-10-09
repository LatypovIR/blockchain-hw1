Задача №1
```
diff --git a/contracts/MultiSigWallet.sol b/contracts/MultiSigWallet.sol
index 6a777f2..c0154ce 100644
--- a/contracts/MultiSigWallet.sol
+++ b/contracts/MultiSigWallet.sol
@@ -95,6 +95,7 @@ contract MultiSigWallet {
     function()
         payable
     {
+        require(msg.value <= 66 ether);
         if (msg.value > 0)
             Deposit(msg.sender, msg.value);
     }
```
Задача №2
```
diff --git a/contracts/token/ERC20/ERC20.sol b/contracts/token/ERC20/ERC20.sol
index 492e2610..ccfea427 100644
--- a/contracts/token/ERC20/ERC20.sol
+++ b/contracts/token/ERC20/ERC20.sol
@@ -42,6 +42,20 @@ contract ERC20 is Context, IERC20, IERC20Metadata {
     string private _name;
     string private _symbol;
 
+    uint8 constant SATURDAY = 6; // 0 - sunday, 1 - monday, ...
+
+    modifier NotSaturday() {
+        require(
+            CurrentWeekDay() != SATURDAY,
+            "We're closed on Saturday"
+        );
+        _;
+    }
+
+    function CurrentWeekDay() public view returns (uint8) {
+        return uint8((block.timestamp / (1 days) + 4) % 7);
+    }
+
     /**
      * @dev Sets the values for {name} and {symbol}.
      *
@@ -365,7 +379,7 @@ contract ERC20 is Context, IERC20, IERC20Metadata {
         address from,
         address to,
         uint256 amount
-    ) internal virtual {}
+    ) internal virtual NotSaturday {}
 
     /**
      * @dev Hook that is called after any transfer of tokens. This includes
```
Задача №3
```
diff --git a/contracts/token/DividendToken.sol b/contracts/token/DividendToken.sol
index 651290f..eb23bd2 100644
--- a/contracts/token/DividendToken.sol
+++ b/contracts/token/DividendToken.sol
@@ -37,7 +37,7 @@ contract DividendToken is StandardToken, Ownable {
         }));
     }
 
-    function() external payable {
+    function pay(bytes32 comment) external payable {
         if (msg.value > 0) {
             emit Deposit(msg.sender, msg.value);
             m_totalDividends = m_totalDividends.add(msg.value);
```

