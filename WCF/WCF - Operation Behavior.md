Operation Behavior
===

- In WCF, there are Microsoft call "extension" points for Endpoint, Contract, Service, and Operation.
- BUT, the mechanism is HORRIBLY described and NOT fully understood by people using WCF. There is not much good on the web about this Behavior topic, and even there are a few, they are not easy to understand, at least for me.



- An Example is worth a thousand words:

> When WCF client tries to save a NEW Account, on the service side, it tries to fill in some date and user information to the NEW Account passed from the client, before the account is saved to the Database, in order to TRACK who and when the account was created.

> Simple example, yet there a lot of things involved that scattered around, making it hard to get a mental picture of the whole thing. It is a great feature, but hard to understand and hard to use. That's may be why WCF is not widely used since its inception.

> In order to the simple filling out creation dates and person's name, which is only about 30 lines of code; yet, there are many things involved.

1. Define an interface ITrackable to have all the 

2. Define the "Behavior Attribute" that inherits Attribute and IOperationBehavior. In ApplyDispatcherBehavior() add a Parameter Inspector (to be created) object to the dispatchOperation.ParameterInspectors' collection.

3. Decorate the Service Operation that need to check if client passing <ITrackable> objects over.

4. Create an Parameter Inspector (class) that implements the IParameterInspector, and implement the BeforeCall(...) in which it look through (inputs) objects, passed by the client, for a specify type of object, (of type <ITrackable>), and fills in the needed information before the <ITrackable> object is saved.

- Summary:

- ITrackable, for Trackable objects
- Behavior Attribute: Attribute, IOperationBehavior
- Decorate Service Operation (method) with Behavior Attribute
- Parameter Inspector:IParameterParameter to look for ITrackable objects to fill in the needed info.

- TODO: summary the use of Behaviors

```csharp
public interface ITrackable
{
    String CreatedBy { get; set; }      // who created
    DateTime CreatedAt { get; set; }    // creation date

    String LastModifiedBy { get; set; } // who last modified it
    DateTime LastModifiedAt { get; set; }   // date of modification

    bool IsNew();   // is New, creation
}
```

```csharp
public class TrackableBehaviorAttribute : Attribute, IOperationBehavior
{
  public void Validate(OperationDescription od){}   // do nothing

  public void ApplyDispatchBehavior(OperationDescription od, DispatchOperation do)
  {
    do.ParameterInspectors.Add(new TrackableParameterInspector());  // add parameter inspector
  }

  public void ApplyClientBehavior(OperationDescription od, ClientOperation co){}// do nothing
  public void AddBindingParameters(OperationDescription od, BindingParameterCollection bp){}// do nothing
}

```

```csharp
public class TrackableParameterInspector : IParameterInspector 
{
    public object BeforeCall(string operationName, object[] inputs)
    {
        if (inputs == null)
            return null;

        var curOC = OperationContext.Current; 

        if (curOC == null
            || curOC.ServiceSecurityContext == null
            || curOC.ServiceSecurityContext.PrimaryIdentity == null)
            return null;

        inputs.OfType<ITrackable>().ToList().ForEach(trackable => 
        { 
            var currentUser = curOC.ServiceSecurityContext.PrimaryIdentity.Name;
            var stampTime = DateTime.UtcNow;

            trackable.LastModifiedBy = currentUser;
            trackable.LastModifiedAt = stampTime;

            if (trackable.IsNew())
            {
                trackable.CreatedBy = currentUser;
                trackable.CreatedAt = stampTime;
            }
        });

        return null;
    }

    public void AfterCall(string operationName, object[] outputs, object returnValue, object correlationState){}
}

```

```csharp
[TrackableBehavior]
[PrincipalPermission(SecurityAction.Demand, Authenticated = true, Role = AppRoles.AssignmentMakers)]
public int SaveAccount(Account toSave, string notes)
{
    try
    {
        if (toSave == null)
            throw new ArgumentNullException(nameof(toSave));

        using (var tran = new TransactionScope())
        {
            toSave = _accountRepository.SaveAccount(toSave);
            //do the audit after the save in case it's new (so we'll have an id)
            _auditService.AuditSave(toSave.Clone(true), notes);
            tran.Complete();
        }

        Debug.Assert(toSave != null);
        Debug.Assert(toSave.Id > 0);

        return toSave.Id;
    }
    catch (NHibernate.StaleObjectStateException)
    {
        throw new FaultException<SaveFailureReason>(SaveFailureReason.StaleObject);
    }
    catch (CircularReferenceException)
    {
        throw new FaultException<SaveFailureReason>(SaveFailureReason.CircularReference);
    }
    catch (Exception ex)
    {
        ExceptionPolicy.HandleException(ex, "ServiceLayor");
        throw;
    }
}
```

```csharp
[DataContract]
[KnownType(typeof(...
...
public class Account : ITrackable, ...
{

  ...
  #region ITrackable Members
  [DataMember]
  public virtual string CreatedBy { get; set; }
  [DataMember]
  public virtual DateTime CreatedAt { get; set; }
  [DataMember]
  public virtual string LastModifiedBy { get; set; }
  [DataMember]
  public virtual DateTime LastModifiedAt { get; set; }

  public virtual bool IsNew()
  {
      return Id == 0;
  }
  #endregion
}
```



- [Ref](https://docs.microsoft.com/en-us/archive/blogs/carlosfigueira/wcf-extensibility-ioperationbehavior)