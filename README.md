# FIFO-of-16-8-memory-verilog-code

module sync_fifo #(parameter WIDTH = 8, DEPTH = 16)(
  
  input 				   clk, rst, wr_en, rd_en,  
  input      [WIDTH-1:0]   data_in,
  
  output reg 			   empty, full,
  output reg [WIDTH-1:0]   data_out
);
  
  parameter PTR_WIDTH = $clog2(DEPTH); //used system task to calculate no. of bits.
 
  reg [WIDTH-1:0] memory [0:DEPTH-1];
 
  reg [PTR_WIDTH:0] rd_ptr, wr_ptr; // width of the ptrs are 1 bit more in order to track the   full and empty conditions, MSB will tell us about FULL and Empty
  
  reg wrap_around ;
  
  //reset condtion - set default values  
 
  always @(posedge clk) 
    begin    
    if(rst) 
      begin      
      rd_ptr <= 0;
      wr_ptr <= 0;
      data_out <= 0;      
      end    
    end
  
  //write condition 
  
  always @(posedge clk)
    begin    
    if( wr_en & !full) 
      begin      
      memory [wr_ptr[PTR_WIDTH-1:0]] <= data_in;
      wr_ptr <= wr_ptr + 1;      
      end    
    end
  
  //read condition
  
  always @(posedge clk) 
    begin    
    if( rd_en & !empty) 
      begin      
      data_out <= memory [rd_ptr[PTR_WIDTH-1:0]];
      rd_ptr <= rd_ptr + 1;      
      end    
   end
  
  assign wrap_around = wr_ptr[PTR_WIDTH]^rd_ptr[PTR_WIDTH]; //if the msb is same then there is no wrap around if msb is diff then there is wrap around.
  
  assign full = (wrap_around) && (rd_ptr[PTR_WIDTH-1:0]==wr_ptr[PTR_WIDTH-1:0]);
  assign empty = (rd_ptr==wr_ptr);
  
endmodule
