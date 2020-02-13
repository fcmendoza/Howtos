# Howtos

See wiki: https://github.com/fcmendoza/Howtos/wiki


```csharp
using System;

namespace Quoter
{
    interface IQuote
    {
        Guid Id { get; set; }
        string Symbol { get; set; }
        double Price { get; set; }
        uint AvailableVolume { get; set; }
        DateTime ExpirationDate { get; set; }
    }

    class Quote : IQuote
    {
        public Guid Id { get; set; }
        public string Symbol { get; set; }
        public double Price { get; set; }
        public uint AvailableVolume { get; set; }
        public DateTime ExpirationDate { get; set; }
    }

    interface ITradeResult
    {
        Guid Id { get; set; }
        string Symbol { get; set; }
        double VolumeWeightedAveragePrice { get; set; }
        uint VolumeRequested { get; set; }
        uint VolumeExecuted { get; set; }
    }

    class TradeResult : ITradeResult
    {
        public Guid Id { get; set; }
        public string Symbol { get; set; }
        public double VolumeWeightedAveragePrice { get; set; }
        public uint VolumeRequested { get; set; }
        public uint VolumeExecuted { get; set; }
    }

    // Please create your own quote manager which implements IQuoteManager interface. 
    // 
    // Do not change the interface.
    // 
    // Please adhere to good Object Oriented Programming concepts, and create whatever support code you feel is necessary.
    //
    // Efficiency counts think about what data structures you use and how each method will perform.
    // 
	// Though not required, feel free to includes any notes on any areas of this interface that you would improve, or which you
	// feel don't adhere to good design concepts or implementation practices. 
    interface IQuoteManager
    {
        // Add or update the quote (specified by Id) in symbol's book. 
        // If quote is new or no longer in the book, add it. Otherwise update it to match the given price, volume, and symbol.
        void AddOrUpdateQuote(IQuote quote);
        
         // Remove quote by Id, if quote is no longer in symbol's book do nothing.
        void RemoveQuote(Guid id);
        
        // Remove all quotes on the specifed symbol's book.
        void RemoveAllQuotes(string symbol);

        // Get the best (i.e. lowest) price in the symbol's book that still has available volume.
        // If there is no quote on the symbol's book with available volume, return null.
        // Otherwise return a Quote object with all the fields set.
        // Don't return any quote which is past its expiration time, or has been removed.
        IQuote GetBestQuoteWithAvailableVolume(string symbol);

        // Request that a trade be executed. For the purposes of this interface, assume that the trade is a request to BUY, not sell. Do not trade an expired quotes.
        // To Execute a trade:
        //   * Search available quotes of the specified symbol from best price to worst price.
        //   * Until the requested volume has been filled, use as much available volume as necessary (up to what is available) from each quote, subtracting the used amount from the available amount.
        // For example, we have two quotes:
        //   {Price: 1.0, Volume: 1,000, AvailableVolume: 750}
        //   {Price: 2.0, Volume: 1,000, AvailableVolume: 1,000}
        // After calling once for 500 volume, the quotes are:
        //   {Price: 1.0, Volume: 1,000, AvailableVolume: 250}
        //   {Price: 2.0, Volume: 1,000, AvailableVolume: 1,000}
        // And After calling this a second time for 500 volume, the quotes are:
        //   {Price: 1.0, Volume: 1,000, AvailableVolume: 0}
        //   {Price: 2.0, Volume: 1,000, AvailableVolume: 750}
        ITradeResult ExecuteTrade(string symbol, uint volumeRequested);
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```
