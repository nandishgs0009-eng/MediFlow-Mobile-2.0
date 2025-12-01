# Code Examples for Remaining Pages

## Dashboard Page - Mobile Optimization Example

### Before (Desktop-Only)
```tsx
const Dashboard = () => {
  return (
    <div className="flex">
      <Sidebar className="w-64" />
      <main className="ml-64 p-8">
        <div className="grid grid-cols-4 gap-8">
          {/* Stats cards always 4 columns */}
        </div>
      </main>
    </div>
  );
};
```

### After (Mobile-First)
```tsx
const Dashboard = () => {
  const [sidebarOpen, setSidebarOpen] = useState(false);

  return (
    <div className="flex flex-col md:flex-row min-h-screen">
      {/* Sidebar - hidden on mobile */}
      <Sidebar 
        isOpen={sidebarOpen}
        onClose={() => setSidebarOpen(false)}
        className="hidden md:flex md:w-64 flex-col fixed md:relative inset-0 md:inset-auto z-40"
      />

      {/* Main Content */}
      <main className="flex-1 w-full overflow-hidden">
        {/* Mobile Header */}
        <div className="md:hidden flex items-center justify-between p-4 bg-card border-b border-border/50">
          <h1 className="text-lg font-bold">Dashboard</h1>
          <button
            onClick={() => setSidebarOpen(!sidebarOpen)}
            className="p-2 hover:bg-secondary rounded-lg"
            aria-label="Toggle menu"
          >
            {sidebarOpen ? <X size={24} /> : <Menu size={24} />}
          </button>
        </div>

        {/* Content - responsive padding */}
        <div className="p-4 sm:p-6 md:p-8">
          {/* Responsive Grid: 1 col mobile → 2 col tablet → 4 col desktop */}
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 sm:gap-6 md:gap-8">
            <StatCard label="Total Medications" value="12" />
            <StatCard label="Today's Doses" value="6" />
            <StatCard label="Adherence" value="94%" />
            <StatCard label="Stock Status" value="Good" />
          </div>

          {/* Charts - responsive height */}
          <div className="mt-6 md:mt-8 grid grid-cols-1 lg:grid-cols-2 gap-4 md:gap-6">
            <Card>
              <CardHeader>
                <CardTitle className="text-lg sm:text-xl">Daily Adherence</CardTitle>
              </CardHeader>
              <CardContent>
                <ResponsiveContainer width="100%" height={300}>
                  <AreaChart data={adherenceData}>
                    <Area type="monotone" dataKey="adherence" stroke="#22c55e" />
                  </AreaChart>
                </ResponsiveContainer>
              </CardContent>
            </Card>

            <Card>
              <CardHeader>
                <CardTitle className="text-lg sm:text-xl">Doses This Week</CardTitle>
              </CardHeader>
              <CardContent>
                <ResponsiveContainer width="100%" height={300}>
                  <BarChart data={weeklyData}>
                    <Bar dataKey="doses" fill="#0ea5e9" />
                  </BarChart>
                </ResponsiveContainer>
              </CardContent>
            </Card>
          </div>

          {/* Today's Medications - responsive list */}
          <Card className="mt-6 md:mt-8">
            <CardHeader>
              <CardTitle className="text-lg sm:text-xl">Today's Medications</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="space-y-3 md:space-y-4">
                {todaysMedicines.map((med) => (
                  <div key={med.id} className="flex flex-col sm:flex-row items-start sm:items-center justify-between p-3 sm:p-4 rounded-lg bg-secondary/50 gap-3 sm:gap-4">
                    <div className="flex-1 min-w-0">
                      <h4 className="font-semibold text-sm md:text-base truncate">{med.name}</h4>
                      <p className="text-xs md:text-sm text-muted-foreground">{med.dose} • {med.time}</p>
                    </div>
                    <Badge className="flex-shrink-0 text-xs md:text-sm">
                      {med.status === 'taken' ? 'Taken' : 'Pending'}
                    </Badge>
                    <Button 
                      size="sm" 
                      className="w-full sm:w-auto text-xs md:text-sm"
                      onClick={() => markAsTaken(med.id)}
                    >
                      Mark Taken
                    </Button>
                  </div>
                ))}
              </div>
            </CardContent>
          </Card>
        </div>
      </main>
    </div>
  );
};
```

---

## Medical Records Page - Mobile Optimization

### Responsive Table/Card View
```tsx
const MedicalRecordsContent = () => {
  const [selectedRecord, setSelectedRecord] = useState<any>(null);

  return (
    <div className="p-4 sm:p-6 md:p-8">
      {/* Search/Filter - full width on mobile */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4 md:gap-6 mb-6 md:mb-8">
        <input
          type="search"
          placeholder="Search records..."
          className="h-11 md:h-10 px-4 md:px-3 border border-input rounded-md text-base md:text-sm"
        />
        <select className="h-11 md:h-10 px-4 md:px-3 border border-input rounded-md text-base md:text-sm">
          <option>All Types</option>
          <option>Prescriptions</option>
          <option>Lab Reports</option>
        </select>
        <Button className="h-11 md:h-10 w-full md:w-auto text-sm md:text-base">
          <Plus className="w-4 h-4 mr-2" />
          Add Record
        </Button>
      </div>

      {/* Desktop: Table View (hidden on mobile) */}
      <div className="hidden md:block overflow-x-auto rounded-lg border border-border">
        <table className="w-full text-sm">
          <thead className="bg-secondary/50 border-b border-border">
            <tr>
              <th className="text-left p-3 font-semibold">Date</th>
              <th className="text-left p-3 font-semibold">Type</th>
              <th className="text-left p-3 font-semibold">Document</th>
              <th className="text-left p-3 font-semibold">Doctor</th>
              <th className="text-left p-3 font-semibold">Actions</th>
            </tr>
          </thead>
          <tbody>
            {records.map((record) => (
              <tr key={record.id} className="border-b border-border hover:bg-secondary/30">
                <td className="p-3">{formatDate(record.date)}</td>
                <td className="p-3">
                  <Badge variant="secondary">{record.type}</Badge>
                </td>
                <td className="p-3 truncate">{record.name}</td>
                <td className="p-3 text-muted-foreground">{record.doctor}</td>
                <td className="p-3">
                  <div className="flex gap-2">
                    <Button variant="ghost" size="sm" onClick={() => viewRecord(record)}>
                      View
                    </Button>
                    <Button variant="ghost" size="sm" onClick={() => downloadRecord(record)}>
                      Download
                    </Button>
                  </div>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>

      {/* Mobile: Card View (hidden on desktop) */}
      <div className="md:hidden space-y-3">
        {records.map((record) => (
          <Card key={record.id} className="p-4">
            <div className="flex justify-between items-start gap-3 mb-3">
              <div className="flex-1 min-w-0">
                <h4 className="font-semibold text-sm truncate">{record.name}</h4>
                <p className="text-xs text-muted-foreground">{formatDate(record.date)}</p>
              </div>
              <Badge variant="secondary" className="flex-shrink-0 text-xs">
                {record.type}
              </Badge>
            </div>
            
            <p className="text-xs text-muted-foreground mb-3">
              By: {record.doctor || 'N/A'}
            </p>
            
            <div className="grid grid-cols-2 gap-2">
              <Button 
                variant="outline" 
                size="sm" 
                className="text-xs h-10"
                onClick={() => viewRecord(record)}
              >
                View
              </Button>
              <Button 
                variant="outline" 
                size="sm" 
                className="text-xs h-10"
                onClick={() => downloadRecord(record)}
              >
                Download
              </Button>
            </div>
          </Card>
        ))}
      </div>
    </div>
  );
};
```

---

## Form Pages - Mobile Optimization

### Responsive Form Layout
```tsx
const MedicineForm = () => {
  return (
    <div className="p-4 sm:p-6 md:p-8 max-w-4xl mx-auto">
      <h1 className="text-2xl sm:text-3xl md:text-4xl font-bold mb-6 md:mb-8">
        Add New Medication
      </h1>

      <form className="space-y-6 md:space-y-8">
        {/* Single column inputs */}
        <div className="space-y-2 md:space-y-3">
          <label className="block text-sm md:text-base font-medium">
            Medicine Name *
          </label>
          <input
            type="text"
            placeholder="e.g., Aspirin"
            className="w-full h-11 md:h-10 px-4 md:px-3 py-2 md:py-1 border border-input rounded-md focus:ring-2 focus:ring-primary"
            required
          />
        </div>

        {/* Two column layout - stacks on mobile */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-6">
          <div className="space-y-2 md:space-y-3">
            <label className="block text-sm md:text-base font-medium">
              Dose *
            </label>
            <input
              type="text"
              placeholder="e.g., 500mg"
              className="w-full h-11 md:h-10 px-4 md:px-3 py-2 md:py-1 border border-input rounded-md"
              required
            />
          </div>

          <div className="space-y-2 md:space-y-3">
            <label className="block text-sm md:text-base font-medium">
              Frequency *
            </label>
            <select className="w-full h-11 md:h-10 px-4 md:px-3 border border-input rounded-md">
              <option>Once daily</option>
              <option>Twice daily</option>
              <option>Thrice daily</option>
              <option>Every 4 hours</option>
            </select>
          </div>
        </div>

        {/* Three column layout - 1 on mobile, 2 on tablet, 3 on desktop */}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
          <div className="space-y-2 md:space-y-3">
            <label className="block text-sm md:text-base font-medium">
              Start Date
            </label>
            <input
              type="date"
              className="w-full h-11 md:h-10 px-4 md:px-3 border border-input rounded-md text-base md:text-sm"
            />
          </div>

          <div className="space-y-2 md:space-y-3">
            <label className="block text-sm md:text-base font-medium">
              End Date (Optional)
            </label>
            <input
              type="date"
              className="w-full h-11 md:h-10 px-4 md:px-3 border border-input rounded-md text-base md:text-sm"
            />
          </div>

          <div className="space-y-2 md:space-y-3">
            <label className="block text-sm md:text-base font-medium">
              Stock
            </label>
            <input
              type="number"
              placeholder="e.g., 30"
              className="w-full h-11 md:h-10 px-4 md:px-3 border border-input rounded-md text-base md:text-sm"
              inputMode="decimal"
            />
          </div>
        </div>

        {/* Notes - full width */}
        <div className="space-y-2 md:space-y-3">
          <label className="block text-sm md:text-base font-medium">
            Notes (Optional)
          </label>
          <textarea
            placeholder="Add any notes about this medication..."
            className="w-full px-4 md:px-3 py-3 md:py-2 border border-input rounded-md text-base md:text-sm min-h-[120px]"
          />
        </div>

        {/* Buttons - stack on mobile, inline on desktop */}
        <div className="flex flex-col sm:flex-row gap-3 md:gap-4 pt-4 md:pt-6">
          <Link to="/medications" className="w-full sm:flex-1">
            <Button 
              variant="outline" 
              className="w-full h-11 md:h-10 text-sm md:text-base"
            >
              Cancel
            </Button>
          </Link>
          <button
            type="submit"
            className="w-full sm:flex-1 h-11 md:h-10 px-4 md:px-6 bg-primary text-primary-foreground rounded-md hover:opacity-90 transition font-medium text-sm md:text-base"
          >
            Add Medication
          </button>
        </div>
      </form>
    </div>
  );
};
```

---

## Profile Page - Mobile Optimization

### Responsive Profile Layout
```tsx
const ProfilePage = () => {
  return (
    <div className="p-4 sm:p-6 md:p-8 max-w-4xl mx-auto">
      {/* Header - responsive */}
      <div className="flex flex-col sm:flex-row items-center sm:items-start gap-4 sm:gap-6 mb-8 md:mb-12">
        <Avatar className="w-20 h-20 md:w-24 h-24">
          <AvatarImage src={profile.avatar} />
          <AvatarFallback>{profile.initials}</AvatarFallback>
        </Avatar>
        
        <div className="flex-1 text-center sm:text-left">
          <h1 className="text-2xl sm:text-3xl md:text-4xl font-bold mb-2">
            {profile.name}
          </h1>
          <p className="text-sm sm:text-base text-muted-foreground mb-4">
            {profile.email}
          </p>
          
          <div className="flex flex-col sm:flex-row gap-2 md:gap-3">
            <Button size="sm" className="text-xs md:text-sm">Edit Profile</Button>
            <Button variant="outline" size="sm" className="text-xs md:text-sm">
              Download Records
            </Button>
          </div>
        </div>
      </div>

      {/* Content Grid - 1 col mobile, 2 col desktop */}
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6 md:gap-8">
        {/* Personal Information */}
        <Card>
          <CardHeader>
            <CardTitle className="text-lg sm:text-xl">Personal Information</CardTitle>
          </CardHeader>
          <CardContent className="space-y-4 md:space-y-6">
            <div>
              <label className="text-xs md:text-sm text-muted-foreground">Age</label>
              <p className="text-sm md:text-base font-medium">{profile.age} years</p>
            </div>
            <div>
              <label className="text-xs md:text-sm text-muted-foreground">Blood Type</label>
              <p className="text-sm md:text-base font-medium">{profile.bloodType}</p>
            </div>
            <div>
              <label className="text-xs md:text-sm text-muted-foreground">Allergies</label>
              <p className="text-sm md:text-base font-medium">{profile.allergies || 'None'}</p>
            </div>
          </CardContent>
        </Card>

        {/* Medical History */}
        <Card>
          <CardHeader>
            <CardTitle className="text-lg sm:text-xl">Medical History</CardTitle>
          </CardHeader>
          <CardContent>
            <div className="space-y-3 md:space-y-4">
              {profile.conditions.map((condition) => (
                <Badge key={condition.id} variant="secondary" className="text-xs md:text-sm">
                  {condition.name}
                </Badge>
              ))}
            </div>
          </CardContent>
        </Card>

        {/* Current Medications - spans full width */}
        <Card className="md:col-span-2">
          <CardHeader>
            <CardTitle className="text-lg sm:text-xl">Current Medications</CardTitle>
          </CardHeader>
          <CardContent>
            <div className="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6">
              {profile.medications.map((med) => (
                <div key={med.id} className="p-3 md:p-4 rounded-lg border border-border/50 bg-secondary/30">
                  <h4 className="font-semibold text-sm md:text-base mb-1">{med.name}</h4>
                  <p className="text-xs md:text-sm text-muted-foreground">{med.dose}</p>
                </div>
              ))}
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
};
```

---

## Notifications Page - Mobile Optimization

### Responsive Notification List
```tsx
const NotificationsPage = () => {
  const [notifications, setNotifications] = useState([]);

  return (
    <div className="p-4 sm:p-6 md:p-8 max-w-4xl mx-auto">
      <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-6 md:mb-8">
        <h1 className="text-2xl sm:text-3xl md:text-4xl font-bold">Notifications</h1>
        
        <Button 
          variant="outline" 
          size="sm"
          onClick={() => markAllRead()}
          className="w-full sm:w-auto text-xs md:text-sm"
        >
          Mark all as read
        </Button>
      </div>

      {/* Notification Filters - responsive tabs */}
      <div className="flex flex-wrap gap-2 md:gap-3 mb-6 md:mb-8 overflow-x-auto pb-2">
        {['All', 'Reminders', 'Refills', 'Updates'].map((tab) => (
          <Button
            key={tab}
            variant={tab === 'All' ? 'default' : 'ghost'}
            size="sm"
            className="flex-shrink-0 text-xs md:text-sm"
          >
            {tab}
          </Button>
        ))}
      </div>

      {/* Notifications List - card view */}
      <div className="space-y-3 md:space-y-4">
        {notifications.map((notif) => (
          <Card 
            key={notif.id} 
            className={`p-4 md:p-6 cursor-pointer transition hover:bg-secondary/50 ${
              !notif.read ? 'border-primary/50 bg-primary/5' : ''
            }`}
            onClick={() => markAsRead(notif.id)}
          >
            <div className="flex flex-col sm:flex-row items-start sm:items-center gap-3 md:gap-4">
              {/* Icon */}
              <div className="flex-shrink-0 w-10 h-10 md:w-12 h-12 rounded-lg bg-primary/10 flex items-center justify-center">
                {notif.type === 'reminder' && <Bell className="w-5 h-5 md:w-6 h-6 text-primary" />}
                {notif.type === 'refill' && <AlertCircle className="w-5 h-5 md:w-6 h-6 text-warning" />}
                {notif.type === 'update' && <CheckCircle2 className="w-5 h-5 md:w-6 h-6 text-success" />}
              </div>

              {/* Content */}
              <div className="flex-1 min-w-0">
                <h4 className="font-semibold text-sm md:text-base mb-1">{notif.title}</h4>
                <p className="text-xs md:text-sm text-muted-foreground mb-2 line-clamp-2">
                  {notif.message}
                </p>
                <p className="text-xs text-muted-foreground">
                  {formatRelativeTime(notif.date)}
                </p>
              </div>

              {/* Unread Indicator */}
              {!notif.read && (
                <div className="flex-shrink-0 w-2 h-2 md:w-3 h-3 rounded-full bg-primary mt-1 sm:mt-0" />
              )}
            </div>

            {/* Actions */}
            <div className="flex flex-col sm:flex-row gap-2 mt-3 md:mt-4 pt-3 md:pt-4 border-t border-border/50">
              <Button 
                variant="ghost" 
                size="sm"
                className="w-full sm:w-auto text-xs md:text-sm"
              >
                View Details
              </Button>
              <Button 
                variant="ghost" 
                size="sm"
                className="w-full sm:w-auto text-xs md:text-sm"
                onClick={() => deleteNotification(notif.id)}
              >
                Delete
              </Button>
            </div>
          </Card>
        ))}
      </div>

      {/* Empty State */}
      {notifications.length === 0 && (
        <div className="text-center py-12 md:py-16">
          <Bell className="w-12 h-12 md:w-16 h-16 text-muted-foreground/50 mx-auto mb-4" />
          <p className="text-sm md:text-base text-muted-foreground">
            No notifications yet
          </p>
        </div>
      )}
    </div>
  );
};
```

---

## Key Principles Applied in All Examples

1. **Mobile-First Approach**
   - Single column by default
   - `md:` prefix for tablet/desktop changes
   - `lg:` prefix for large desktop layouts

2. **Responsive Typography**
   - Base font: `text-sm md:text-base` for body
   - Headings scale: `text-lg sm:text-xl md:text-2xl`
   - Always minimum 16px on mobile

3. **Touch-Friendly**
   - Buttons: `h-11 md:h-10` (44px on mobile)
   - Inputs: `h-11 md:h-10` (44px on mobile)
   - Interactive areas always > 44x44px

4. **Responsive Spacing**
   - `p-4 sm:p-6 md:p-8` (16px, 24px, 32px)
   - `gap-4 md:gap-6` for grid spacing
   - `space-y-4 md:space-y-6` for stacking

5. **Grid Layouts**
   - `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3`
   - Starts with 1 column on mobile
   - Expands responsively

6. **Hide/Show Patterns**
   - `hidden md:block` - hide on mobile
   - `md:hidden` - hide on desktop
   - Different UI patterns for different sizes

---

These examples follow all best practices and can be applied to any remaining page in the application.
