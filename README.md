# BOLLINGER BAND SIMPLE ALGO FOR AAPL STOCK (DOESN'T INCORPORATE LEVERAGE FOR SHORTING)
#RUNS ON QUANTOPIAN PLATFORM



def initialize (context):
    
    context.aa = sid(24)
    
    schedule_function(check_bands, date_rules.every_day())
    
    
    
def check_bands(context, data):
    
     cur_price = data.current(context.aa, 'price')
        
     prices = data.history(context.aa, 'price', 20, '1d')
    
     avg = prices.mean()
     
     std = prices.std()
     
     lower_band = avg - 2*std
    
     upper_band = avg + 2*std
    
    
     if cur_price <= lower_band:
        
        order_target_percent(context.aa,1.0)
        print("Buying")
        
     elif cur_price >= upper_band:
        
        order_target_percent(context.aa,-1.0)
        
        print ("Short Sell")
        
    
    
     else: 
        pass
    
    
     record(upper = upper_band,
           lower = lower_band,
           mavg_20 = avg,
           price = cur_price)
