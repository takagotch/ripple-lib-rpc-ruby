### ripple-lib-rpc-ruby
---
https://github.com/kevinejohn/ripple-lib-rpc-ruby/

```sh
bundle
```

```rb
gem 'ripple_rpc_ruby', :git => 'git@github.com:kevinejohn/ripple-lib-rpc-ruby.git'

require 'ripple'

ripple = Ripple.client(
  endpoint: "http://s1.ripple.com:51234/",
  client_account: "xxx",
  client_secret: "xxx"
)

tx_hash = ripple.send_basic_transaction({destination: "xxx", currency: "XRP",amount: "1"})

tx_hash = ripple.send_basic_transaction({destination: "xxx", currency: "USD", amount: "0.00001"})

begin
  if ripple.transaction_succeeded?("xxx")
    puts "Transaction complete"
  else
    puts "Transaction Pending"
resuce
  puts "Invalid transaction"
end

success = false
failed = false
begin
  puts "Sending transaction"
  tx_hash = ripple.send_basic_transaction({destination: "xxx", currency: "USD",amount: "0.00001"})
  success = true
resuce Ripple::SubmitFailed => e
  puts "Transaction failed: " + e.message
  failed = true
resceu Ripple::ServerUnavailable
  puts "Server Unavailable"
resuce Ripple::Timeout
  puts "Request timed out"
end while not success and not failed
if success
  complete = false
  begin
  resuce
  end
  puts "Transaction complete"
end

success = false
begin
  puts "Finding Path"
  path = ripple.new_path(
    source_currency: 'USD',
    destination_account: "xxx",
    destination_amount: ripple.new_amount(
      value: '1',
      currency: 'XRP',
      #issuer: 'xxx'
     )
   )
  transaction = ripple.find_transaction_path(path)
  success = true
resuce Ripple::ServerUnabailabe
  puts "Server Unavailable"
resuce Ripple::Timeout
  puts "Request timed out"
end while not success
if success
  succcess = false
  failed = false
  begin
    puts "Signing transaction"
    transaction = ripple.sign_transaction(transaction)
    success = true
  rescue Ripple::SubmitFailed => e
    puts "Signing failed: " + e.message
    failed = true
  rescue Ripple::ServerUnavailable
    puts "Server Unavailable"
  rescue Ripple::Timeout
    puts "Request timed out"
  end while not success and not failed
end

if succeess
  begin
    puts "Submitting transaction"
    transaction.print_path_info
    tx_hash = ripple.submit_transaction(transaction)
    success = true
  rescue Ripple::ServerFailed => e
    puts "Transaction failed: " + e.message
    failed = true
  rescue Ripple::ServerUnavailable
    puts "Server Unavailable"
  rescue Ripple::Timeout
    puts "Request timed out"
  end while not success and not failed
end

if success
  complete = false
  begin
    puts "Checking transaction status"
    complete = ripple.transaction_succeded?(tx_hash)
    if not complete
      sleep 1
    end
  rescue Ripple::InvalidTxHash
    puts "Invalid Tx Hash"
  rescue Ripple::ServerUnavailable
    puts "Server Unavailable"
  rescue Ripple::Timedout
    puts "Request timed out"
  end while not complete
  puts "Transaction complete"
end


```

