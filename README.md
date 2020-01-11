# Web config

    <connectionStrings>
    <add name= "DBConnectionString" connectionString="Data Source=Media.ruppin.ac.il;
      Initial Catalog=igroup18_test1; User ID=igroup18; Password=igroup18_08824"/>
    </connectionStrings>
  
  #Controller
  
          public class DiscountsController : ApiController
    {

        [HttpGet]

        public List<Discounts> Getdiscounts()
        {
            Discounts discount = new Discounts();
            return discount.getdiscounts();
        }

        // GET api/<controller>/5
        public string Get(int id)
        {
            return "value";
        }

        // POST api/<controller>
        public List<Discounts> Post(Discounts discount1)
        {
            Discounts discount = new Discounts();
            discount1.InsertDiscount();
            return discount.getdiscounts();
        }
        // PUT api/<controller>/5
        public List<Discounts> Put(Discounts discount2)
        {
            Discounts discount = new Discounts();
            discount2.PutDiscount();
            return discount.getdiscounts();
        }
        // DELETE api/<controller>/5
        public List<Discounts> Delete(int id)
        {
            Discounts discount = new Discounts();
            discount.DeleteDiscount(id);
            return discount.getdiscounts();
        }
    }
  
 # Model
        public class DiscountsController : ApiController
    {

        [HttpGet]

        public List<Discounts> Getdiscounts()
        {
            Discounts discount = new Discounts();
            return discount.getdiscounts();
        }

        // GET api/<controller>/5
        public string Get(int id)
        {
            return "value";
        }

        // POST api/<controller>
        public List<Discounts> Post(Discounts discount1)
        {
            Discounts discount = new Discounts();
            discount1.InsertDiscount();
            return discount.getdiscounts();
        }
        // PUT api/<controller>/5
        public List<Discounts> Put(Discounts discount2)
        {
            Discounts discount = new Discounts();
            discount2.PutDiscount();
            return discount.getdiscounts();
        }
        // DELETE api/<controller>/5
        public List<Discounts> Delete(int id)
        {
            Discounts discount = new Discounts();
            discount.DeleteDiscount(id);
            return discount.getdiscounts();
        }
    }
    
   # DBservices
    
        public List<Discounts> getdiscounts()
    {
        List<Discounts> discounts = new List<Discounts>();
        SqlConnection con = null;


        try
        {
            con = connect("DBConnectionString"); // create the connection
        }
        catch (Exception ex)
        {
            // write to log
            throw (ex);
        }
        try
        {
            String selectSTR = "SELECT * FROM Discounts";
            SqlCommand cmd = new SqlCommand(selectSTR, con);

            // get a reader
            SqlDataReader dr2 = cmd.ExecuteReader();// (CommandBehavior.CloseConnection); // CommandBehavior.CloseConnection: the connection will be closed after reading has reached the end

            while (dr2.Read())
            {   // Read till the end of the data into a row
                Discounts u = new Discounts();

                u.Airline = (string)dr2["airline"];
                u.Flyfrom = (string)dr2["flyfrom"];
                u.Flyto = (string)dr2["flyto"];
                u.Startdate = (string)dr2["startdate"];
                u.Finishdate = (string)dr2["finishdate"];
                u.Discount = (decimal)dr2["discount"];
                u.Id = (int)dr2["ID"];

                discounts.Add(u);
            }
            dr2.Close();
        }
        catch (Exception ex)
        {
            // write to log
            throw (ex);
        }
        finally
        {
            if (con != null)
            {
                con.Close();
            }
        }
        return discounts;
    }

    //  DELETE Discount
    //--------------------------------------------------------------------------------------------------
    public int DeleteDiscount(int Id)
    {
        SqlConnection con;
        SqlCommand cmd;
        try
        {
            con = connect("DBConnectionString"); // create the connection
        }
        catch (Exception ex)
        {
            // write to log
            throw (ex);
        }
        String cStr = "delete from Discounts where ID = " + Id.ToString();      // helper method to build the insert string
        cmd = CreateCommand(cStr, con);             // create the command
        try
        {
            int numEffected = cmd.ExecuteNonQuery(); // execute the command
            return numEffected;
        }
        catch (Exception ex)
        {
            return 0;
            // write to log
            throw (ex);
        }
        finally
        {
            if (con != null)
            {
                // close the db connection
                con.Close();
            }
        }
    }

    public int InsertDiscount (Discounts discount1)
    {

        SqlConnection con;
        SqlCommand cmd;

        try
        {
            con = connect("DBConnectionString"); // create the connection
        }
        catch (Exception ex)
        {
            // write to log
            throw (ex);
        }

        String cStr = BuildInsertCommandDiscount(discount1);      // helper method to build the insert string

        cmd = CreateCommand(cStr, con);             // create the command

        try
        {
            int numEffected = cmd.ExecuteNonQuery(); // execute the command

            return numEffected;

        }
        catch (Exception ex)
        {
            return 0;
            // write to log
            throw (ex);
        }

        finally
        {
            if (con != null)
            {
                // close the db connection
                con.Close();
            }
        }

    }

    private String BuildInsertCommandDiscount(Discounts discount1)
    {
        String command;

        StringBuilder sb = new StringBuilder();
        sb.AppendFormat("Values('{0}', '{1}', '{2}','{3}','{4}','{5}')", discount1.Airline, discount1.Flyfrom, discount1.Flyto, discount1.Startdate, discount1.Finishdate, discount1.Discount);

        // use a string builder to create the dynamic string
        String prefix = "INSERT INTO Discounts" + "(airline,flyfrom,flyto,startdate,finishdate,discount) ";
        command = prefix + sb.ToString();

        return command;
    }

    public int PutDiscount(Discounts d)
    {
        SqlConnection con;
        SqlCommand cmd;

        try
        {
            con = connect("DBConnectionString"); // create the connection
        }
        catch (Exception ex)
        {
            // write to log
            throw (ex);
        }
        String cStr = BuildPutCommandDiscount(d);      // helper method to build the insert string
        cmd = CreateCommand(cStr, con);             // create the command
        try
        {
            int numEffected = cmd.ExecuteNonQuery(); // execute the command
            return numEffected;
        }
        catch (Exception ex)
        {
            return 0;
            // write to log
            throw (ex);
        }
        finally
        {
            if (con != null)
            {
                // close the db connection
                con.Close();
            }
        }
    }
    //--------------------------------------------------------------------
    // 17.  Build PUT Sale Command
    //--------------------------------------------------------------------
    private String BuildPutCommandDiscount(Discounts discount)
    {
        String command;
        StringBuilder sb = new StringBuilder();
        // use a string builder to create the dynamic string
        String prefix = "UPDATE Discounts SET [airline] = '" + discount.Airline + "', [flyfrom] = '" + discount.Flyfrom + "', [flyto] = '" + discount.Flyto + "', [startdate] = '" + discount.Startdate + "', [finishdate] =  '" + discount.Finishdate + "', [discount] =  " + discount.Discount + " WHERE [ID] = " + discount.Id + "";
        command = prefix;
        return command;
    }
